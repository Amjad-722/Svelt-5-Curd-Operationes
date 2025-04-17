<script>
  let people = $state([]);
  let showModal = $state(false);
  let searchTerm = $state("");
  let sortField = $state("name");
  let sortDirection = $state("asc");

  // Form fields
  let newFirstName = $state("");
  let newLastName = $state("");
  let newEmail = $state("");
  let editingId = $state(null);

  // Derived state
  let filteredPeople = $derived(
    people.filter((person) => {
      const searchLower = searchTerm.toLowerCase();
      const fullName = `${person.firstName} ${person.lastName}`.toLowerCase();
      return (
        fullName.includes(searchLower) ||
        person.email.toLowerCase().includes(searchLower)
      );
    })
  );

  let sortedPeople = $derived(
    [...filteredPeople].sort((a, b) => {
      let comparison = 0;

      if (sortField === "name") {
        const nameA = `${a.lastName}, ${a.firstName}`.toLowerCase();
        const nameB = `${b.lastName}, ${b.firstName}`.toLowerCase();
        comparison = nameA.localeCompare(nameB);
      } else {
        comparison = a[sortField]
          .toLowerCase()
          .localeCompare(b[sortField].toLowerCase());
      }

      return sortDirection === "asc" ? comparison : -comparison;
    })
  );

  function addPerson() {
    if (!newFirstName || !newLastName || !newEmail) return;

    people = [
      ...people,
      {
        id: Date.now(),
        firstName: newFirstName,
        lastName: newLastName,
        email: newEmail,
      },
    ];

    resetForm();
    showModal = false;
  }

  function updatePerson() {
    if (!newFirstName || !newLastName || !newEmail) return;

    people = people.map((person) =>
      person.id === editingId
        ? {
            ...person,
            firstName: newFirstName,
            lastName: newLastName,
            email: newEmail,
          }
        : person
    );

    resetForm();
    editingId = null;
    showModal = false;
  }

  function editPerson(person) {
    newFirstName = person.firstName;
    newLastName = person.lastName;
    newEmail = person.email;
    editingId = person.id;
    showModal = true;
  }

  function deletePerson(id) {
    people = people.filter((person) => person.id !== id);
  }

  function resetForm() {
    newFirstName = "";
    newLastName = "";
    newEmail = "";
  }

  function toggleSort(field) {
    if (sortField === field) {
      sortDirection = sortDirection === "asc" ? "desc" : "asc";
    } else {
      sortField = field;
      sortDirection = "asc";
    }
  }

  function closeModal() {
    resetForm();
    editingId = null;
    showModal = false;
  }
</script>

<div class="min-h-screen bg-gray-50 p-8">
  <div class="max-w-6xl mx-auto bg-white rounded-xl shadow-md overflow-hidden">
    <!-- Header -->
    <div class="bg-gradient-to-r from-blue-500 to-indigo-600 p-6 text-white">
      <h1 class="text-3xl font-bold mb-4">People Manager</h1>

      <div
        class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4"
      >
        <div class="relative w-full sm:w-96">
          <input
            bind:value={searchTerm}
            placeholder="Search people..."
            class="w-full py-2 px-4 pr-10 rounded-lg text-gray-800 focus:outline-none focus:ring-2 focus:ring-blue-300"
          />
          <svg
            class="absolute right-3 top-2.5 h-5 w-5 text-gray-400"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
            />
          </svg>
        </div>

        <button
          on:click={() => {
            resetForm();
            editingId = null;
            showModal = true;
          }}
          class="bg-white text-blue-600 hover:bg-blue-50 font-medium py-2 px-6 rounded-lg transition duration-200 flex items-center gap-2 shadow-sm"
        >
          <svg
            class="h-5 w-5"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M12 6v6m0 0v6m0-6h6m-6 0H6"
            />
          </svg>
          Add Person
        </button>
      </div>
    </div>

    {#if showModal}
      <div
        class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4"
      >
        <div class="bg-white rounded-xl shadow-xl w-full max-w-md">
          <div class="p-6">
            <div class="flex justify-between items-center mb-4">
              <h2 class="text-xl font-semibold text-gray-800">
                {editingId ? "Edit Person" : "Add New Person"}
              </h2>
              <button
                on:click={closeModal}
                class="text-gray-400 hover:text-gray-500"
              >
                <svg
                  class="h-6 w-6"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M6 18L18 6M6 6l12 12"
                  />
                </svg>
              </button>
            </div>

            <div class="space-y-4">
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1"
                  >First Name</label
                >
                <input
                  bind:value={newFirstName}
                  placeholder="First name"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1"
                  >Last Name</label
                >
                <input
                  bind:value={newLastName}
                  placeholder="Last name"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
                />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700 mb-1"
                  >Email</label
                >
                <input
                  bind:value={newEmail}
                  placeholder="Email"
                  type="email"
                  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
                />
              </div>
            </div>

            <div class="flex justify-end gap-3 mt-6">
              <button
                on:click={closeModal}
                class="px-4 py-2 text-gray-700 bg-gray-100 hover:bg-gray-200 rounded-lg transition duration-200"
              >
                Cancel
              </button>
              <button
                on:click={editingId ? updatePerson : addPerson}
                disabled={!newFirstName || !newLastName || !newEmail}
                class="px-4 py-2 text-white bg-blue-600 hover:bg-blue-700 rounded-lg transition duration-200 disabled:opacity-50 disabled:cursor-not-allowed"
              >
                {editingId ? "Update" : "Add"} Person
              </button>
            </div>
          </div>
        </div>
      </div>
    {/if}

    <div class="overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th
              on:click={() => toggleSort("name")}
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider cursor-pointer hover:bg-gray-100"
            >
              <div class="flex items-center">
                Name
                {#if sortField === "name"}
                  <svg
                    class={`ml-1 h-4 w-4 ${sortDirection === "asc" ? "rotate-180" : ""}`}
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M19 9l-7 7-7-7"
                    />
                  </svg>
                {/if}
              </div>
            </th>
            <th
              on:click={() => toggleSort("email")}
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider cursor-pointer hover:bg-gray-100"
            >
              <div class="flex items-center">
                Email
                {#if sortField === "email"}
                  <svg
                    class={`ml-1 h-4 w-4 ${sortDirection === "asc" ? "rotate-180" : ""}`}
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M19 9l-7 7-7-7"
                    />
                  </svg>
                {/if}
              </div>
            </th>
            <th
              class="px-6 py-3 text-right text-xs font-medium text-gray-700 uppercase tracking-wider"
            >
              Actions
            </th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          {#if sortedPeople.length === 0}
            <tr>
              <td colspan="3" class="px-6 py-4 text-center text-gray-700">
                {searchTerm
                  ? "No matching people found"
                  : "No people added yet"}
              </td>
            </tr>
          {:else}
            {#each sortedPeople as person (person.id)}
              <tr class="hover:bg-gray-50">
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <div
                      class="flex-shrink-0 h-10 w-10 bg-blue-100 rounded-full flex items-center justify-center text-blue-600 font-medium"
                    >
                      {person.firstName.charAt(0)}{person.lastName.charAt(0)}
                    </div>
                    <div class="ml-4">
                      <div class="text-sm font-medium text-gray-900">
                        {person.firstName}
                        {person.lastName}
                      </div>
                    </div>
                  </div>
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">
                  {person.email}
                </td>
                <td
                  class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium"
                >
                  <button
                    on:click={() => editPerson(person)}
                    class="text-indigo-600 hover:text-indigo-900 mr-4"
                  >
                    Edit
                  </button>
                  <button
                    on:click={() => deletePerson(person.id)}
                    class="text-red-600 hover:text-red-900"
                  >
                    Delete
                  </button>
                </td>
              </tr>
            {/each}
          {/if}
        </tbody>
      </table>
    </div>
  </div>
</div>

<style>
  /* Add smooth transition for modal */
  .modal-enter,
  .modal-leave-to {
    opacity: 0;
  }
  .modal-enter-active,
  .modal-leave-active {
    transition: opacity 200ms ease;
  }
</style>
