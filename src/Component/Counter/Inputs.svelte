<script>

    let message=$state('');
</script>
<div class="flex flex-col  items-center mt-10 justify-center ">

    <input class="border-2  border-gray-900 rounded-lg py-2 px-2"  bind:value={message} placeholder="write a message"/>
    <p>{message}</p>
</div>