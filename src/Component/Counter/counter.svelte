<script>

    let count =$state(1);
    let double =$derived (count * 3);
    let minus =$derived (double - 2);

//props
    let {number,name} = $props ()

    
    
    $effect(() => {

        if(minus == 37) {
            alert("its ti high");
        }
        
    })

    function hundleClick() {
        count += 1;
    }


</script>
<div class="flex flex-col justify-center items-center">
    <button class="p-4 border-2" onclick={hundleClick}>
{count}
    </button>
    <p> {count} * 3 :{ double  === 3 ? "its even" : "its odd"} </p>
    <p> {double} - 2 :{minus}</p>
    <p class="mt-20 font-extrabolds text-2xl">The number is {number}</p>
    <h1 class="text-2xl font-semibold pt-8">
        this is my {name}
    </h1>
</div>