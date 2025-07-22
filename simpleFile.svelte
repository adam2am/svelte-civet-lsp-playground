<script lang="civet">

switch response
        {status: 100, body} // status:100 doesnt get highlighted
            console.log body
        {status}
            throw Error `status ${status}`

</script>
{bcds}