<script>
  import Counter from "../Component/Counter/counter.svelte";
  import CrudOperationes from "../Component/Counter/crudOperationes/crudOperationes.svelte";
  import IfLogic from "../Component/Counter/ifLogic.svelte";
  import Inputs from "../Component/Counter/Inputs.svelte";

 

 
  const Detail = {
    name: "amjad",
    number: 22,
  };
</script>
<div class="gap-4">

	<CrudOperationes/>
	<hr class="border-1 border-gray-800">
	<Counter {...Detail} />
	<hr class="border-1 border-gray-800">
	<IfLogic />
	<hr class="border-1 border-gray-800">
	<Inputs/>
</div>




