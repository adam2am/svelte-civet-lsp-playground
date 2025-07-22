<script lang="civet">
	abc := 231
	check1 := `${
	value
	}`
	export { abc }
 
	msg3 .= `${ name1 } says hi`
	# vs 
	msg2 .= "#{ name1 } says hi"


	obj :=
		computed: [
		...[1...50].map((r:number) -> r * r)                   // range in spread
		]

	unless a is b
		console.log 'a isnt b'

	obj2 :=
		computed: [
		...[1...50].map((x:string) -> x * x)                   // range in spread
		]
	

	import LiveDemo from "$lib/LiveDemo.svelte";
	LiveDemo // hover on string reference
	// chinese variable go-to check
	中文变量 := 'asd'
	{中文变量}


	// 1 - Simple function in Civet syntax
	add := (a: number, b: number): number => a + b
	foo :=
		bar2: 123
	
	abc23 := foo.bar2 
	[1 .. 2]

	// 2 - Basic array operations
	processArray := (arr: number[]): number[] =>
		arr
			.filter (n) => n > 0
			.map (n) => n * 2

	// 3 - Conditional expression
	getStatus := (value: number): string =>
		if value > 10
			"high"
		else
			"low"

	// 4 - Simple object creation
	createItem := (id: number): object =>
		{ id, name: `Item ${id}`, active: id % 2 is 0 }

	// 5 - Nested function call
	somethingNested := (arr: number[]): number[] =>
		do {
			a .= 1
			do {
				b .= a + 2
				do {
					c .= b + 3
					do {
						console.log(c)
					}
				}
			}
			arr
		}

	letterScheck := ['a'...'d']
	letters := ['a'.."z #{user}"]    // inclusive range '' vs "" string as well

	nestedTpl := `outer ${`inner ${foo2}`}`                 // nested interpolation
	something := 
		here : `123`
	abc := something.here

	inactiveUsers := $derived sortedUsers.filter (user) => not user.isActive
	blockStrVanila := ```
		No indent here
		${"innerCoffeInsideVanilla #{foo1}"} here
		#{obj?.prop}
		#{something.here}
	```    
	blockStrCoffee := """
		No indent here
		#{`innervanillaInsideCoffee ${foo2}`} here
		#{obj?.prop}
		#{something.here}
	"""
	
	// zero-indent block
	inlineCheck := `one-liner ${xdb} here`

	// ─────────── Comments & literals interplay ───────────
	regex := /\/\/ not a comment/                         ### // regex with // ###
	str := "// also not a comment"                        // string with //
	tpl := `// ${x} still not comment`                    // template with //

	// ─────────── Range & slice edge cases ───────────
	degenRange := [x..x]                                  // same start/end
	// fucked here, should say arr is not found
	spreadSlice := [...arr[1..-1]]                       // spread + slice
	mixedRange := [1...3] and [4...6]                          // .. and ... mixed
	
	sliceFunc := -> 
		some1 := arr[-2..-1]                   // negative slice assign
		some2 := [1...3] = [] 

	// what does it communicate.o ookay, moving on
	// unexpected, but not a blocker
	x .= 3 
	multiLineInter2 := ```
		outer line
		${
			"inner " + valueLinter
		}
	```
	for something, i in somethings
		console.log something, i
	
	for something2, i of somethings
		console.log something2, i


	func2 := () ->
		some1 := 123

	switch xdd
		{status: 200, body}
			console.log body
		{status}
			throw Error `status ${status}`
	// ─────────── Pattern matching depth ───────────
	nestedSwitch := 
		switch outer
			{type: "a"}
			then switch inner                                     // nested switch
				{x: 1} then "one"
				{x: 2} then "two"
			{type: "b", value}

	// ─────────── effect rune ───────────
	$effect: -> console.log count                        // effect rune

	// ─────────── Block comment with interpolation ───────────
	###
	This #{should} not map
	at all ${neither} this
	###

	obj23 :=
		computed: [
		...[1...50].map((range) -> range * range)                   // range in spread
		]
	
	a2123 := 21
	// ─────────── Chained property access ───────────
	result3 := ->                                      // optional chain
		['complex key']                                   // bracket notation
		.#a21 
		.-1                                                // negative index
		.slice(1)
		.map(x -> x * x)
		.filter(xada -> xada % 2 == 0)
		.reduce((a, b) -> a + b, 0)
		.sort((a, b) -> a - b)
		.reverse()
	
	inactiveUsers := $derived sortedUsers.filter (user) => not user.isActive

	funcWithVars := (a:number) ->
		abc := 2 * ad

	// ───────────── Template strings & interpolation ─────────────
	greet := `Hello, ${user}!`                         // basic interpolation
	warning := `⚠️  ${level ?? 'LOW'}`                // interpolation + nullish
	b := 3
	раздва := () ->
		console.log 'asfsf'
	// ───────────── Operators / aliases ─────────────
	inc := (x) => x + 1                               // thin arrow
	logger := (msg) -> console.log msg               // thick arrow (->)
	flag := a and b or c                              // aliased && / ||
	numetricCompare := if 31 isnt b then 2
	assert := value is expected                       // aliased ===

	// ───────────── Await helpers ─────────────
	settled := await.allSettled promises              // await operator variant
	fastest  := await.race      promises              // another variant

	// ───────────── Range & slicing ─────────────
	slice5 := numbers[1...3]                    // slice w/ negative end
	slice   := numbers[1...-1]                    // slice w/ negative end

	// ───────────── Pipe operators ─────────────
	normalized := data
		|> .trim()
		|> .toLowerCase()
		|> await

	async function getUsers(fetch: typeof globalThis.fetch): Promise<User[]>
		try
			console.log 'Loading layout data...'
			// 🌟 Beautiful pipe operator for clean data flow
			users := 'https://jsonplaceholder.typicode.com/users'
				|> fetch
				|> await
				|> (response: Response) => 
					if not response.ok
						throw new Error("HTTP #{response.status}: #{response.statusText}")
					response
				|> .json()
				|> await
				|> .slice 0, 8 // for demo
				|> .map (user:string) -> user
				// |> .json()
				// |> await
				// |> .slice 0, 8 // for demo
				// |> .map (user:string) -> ("HTTP #{user.status}: #{user.statusText}")
			return users
		catch error
			console.error 'Failed to load users:', error
			throw error // Re-throw to be caught by #await

	export load := ({ fetch }: { fetch: typeof globalThis.fetch }) ->
		{
			streamed: {
				users: getUsers(fetch)
			}
		}
	// ───────────── Pattern matching switch ─────────────
	switch response
		{status: 200, body}
			console.log body
		{status}
			throw Error `status ${status}`
	// ───────────── Loops ─────────────
	while count < 5                                   // classic while
		count++

	result .= [123, 321]
	result = [...records] switch               // spread + postfix switch
		when 'none' then 0
		else 1
		
	abcc := 321
	if 32 is not abcc


	user .= `123`
		// ───────────── Block strings ─────────────
	multiline := """
	Dear #{user2},
	This is a multi-line string.
	"""
	###
do i believe people over there want to say something to enhance my life, 
actually acts with my interest in mind?
matches => now wanna move
== without ... would be over 50 years ago
	###

	multiline2 .= ```
	dear ${user1}
	```

	// -------------------------------------------------------------
	//  Stress-test fixture – advanced interpolation / operators
	// -------------------------------------------------------------
	value .= `something is here`
	// 1. Cross-line interpolation
	multiLineInter := ```
	outer line
	${
		"inner " + valueLinter
	}
	```
	check := `${
	value
	}`



	# comment check here what it is
	// 2. Interpolation that contains brace objects
	objExpr := `val ${ flag ? {a: 1} : {b: 2} } end here`

	// 23. Depth-3 nested interpolation, same line
	deepNest := `a ${`b ${ `c ${d}` }` }`
	func := (foo:string) -> foo
	// here it is without things
	some := `foo (
		abc
		`
	###
		()
		()
	###
	# ------(

	# top-level comment
		# indented    (still a comment)
	x := 1  # trailing after code
	arr[0]  # trailing after bracket

	# -------------------------------
	# Expected: NOT a comment
	# -------------------------------

	# 1) dot-hash shorthand / private fields
	array.1#     # ← the hash itself should be not a comment
	object.#private
	something .= foo.2#   

	value .= floor # / 2 # 2) hash followed by slash (divide by)

	# 3) interpolation opening  (handled by separate rule)
	msg .= "#{ name1 } says hi"
	
	something .= `${msg}`
	# 4) triple-hash block comment (handled by other rule, not our single-line rule)
	### 
	Everything in here is a block comment.
	### 
	// .derived

</script>

<ul>
{#each items as item (item.id)}
	<li>{item.name}</li>
{/each}
</ul> 