

<script>
  import Counter from "../Component/Counter/counter.svelte";

	let people = $state([
		
	]);

	let prefix = $state('');
	let first = $state('');
	let last = $state('');
	let i = $state(0);

   

	let filteredPeople = $derived(prefix
		? people.filter((person) => {
				const name = `${person.last}, ${person.first}`;
				return name.toLowerCase().startsWith(prefix.toLowerCase());
			})
		: people);
	let selected = $derived(filteredPeople[i]);

	$effect(() => {
		reset_inputs(selected);
	});

	function create() {
    people = people.concat({ first, last });
    i = people.length - 1;
    
   
    first = '';
    last = '';
}



	function update() {
		selected.first = first;
		selected.last = last;
		people = people;
	}

	function remove() {
		
		const index = people.indexOf(selected);
		people = [...people.slice(0, index), ...people.slice(index + 1)];

		first = last = '';
		i = Math.min(i, filteredPeople.length - 2);
	}

	function reset_inputs(person) {
		first = person ? person.first : '';
		last = person ? person.last : '';
	}
</script>

<div><Counter number={23}/></div>

<div class="h-screen  flex flex-col justify-center items-center gap-4">
    <label><input bind:value={first} placeholder="first" class="border-2 border-gray-900 rounded-lg py-2 px-2" /></label>
    <label><input bind:value={last} placeholder="last" class="border-2 border-gray-900 rounded-lg py-2 px-2" /></label>
   

    <select bind:value={i} size={5} class="border-2 border-gray-900 rounded-lg w-1/3">
        {#each filteredPeople as person, i}
            <option value={i}>{person.last}, {person.first}</option>
        {/each}
    </select>
    
  
    <div class="flex flex-row gap-20">
        <input placeholder="filter prefix" bind:value={prefix}  class="border-2 border-gray-900 rounded-lg py-2"/>

        <div class="">
            <button onclick={create} disabled={!first || !last} class="border-2 border-gray-900 rounded-lg p-1.5">create</button>
            <button onclick={update} disabled={!first || !last || !selected} class="border-2 border-gray-900 rounded-lg p-1.5">update</button>
        <button onclick={remove} disabled={!selected} class="border-2 border-gray-900 rounded-lg p-1.5">delete</button>
    </div>
    
</div>
</div>

<div class="sss"><Counter/></div>

