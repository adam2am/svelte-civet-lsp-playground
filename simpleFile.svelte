<script lang="civet">

switch response // response not being properly hovered, linter error only 1st char
        {status: 100, body} // status:100 doesnt get highlighted
            console.log body
        {status}
            throw Error `status ${status}`

</script>


{bcds}