<script>
  let user = $state({ loginIn:false });

  function Toggle() {
    user.loginIn = !user.loginIn;
  }
</script>
<div class="flex justify-center mt-20">


{#if user.loginIn}
<div>
  <button class="border-2 border-neutral-900 p-2 rounded-lg shadow-lg shadow-gray-700  text-blue-600" onclick={Toggle}> Amjad you are login in </button>
</div>
{/if}
{#if !user.loginIn}
<div>
<button onclick={Toggle}> Amjad ayour are log out </button>
</div>
{/if}
</div>
