<script>
  let user = $state({ loginIn: false });

  function Toggle() {
    user.loginIn = !user.loginIn;
  }
  $effect(() => {
    if (user.loginIn) {
      alert("you are log out");
    }
  });
</script>
<div class="flex flex-col justify-center items-center mt-10">

    {#if user.loginIn}
      <button
        class="border-2 p-2 rounded-lg shadow-lg shadow-gray-700 text-blue-600"
        onclick={Toggle}
      >
        Amjad you are login
      </button>
    {:else}
      <button
        class="border-2 p-2 rounded-lg shadow-lg shadow-gray-700 text-red-600"
        onclick={Toggle}
      >
        Opps! Amjad you are log out
      </button>
    {/if}
</div>
