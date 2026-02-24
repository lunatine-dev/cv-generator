<script>
    let jsonData = null;
    let selectedCV = "general";
    let fileName = "";

    const cvOptions = ["general", "nodejs", "python", "frontend", "backend", "fullstack"];

    function handleFileChange(event) {
        const file = event.target.files[0];
        if (!file) return;
        fileName = file.name;

        const reader = new FileReader();
        reader.onload = (e) => {
            try {
                jsonData = JSON.parse(e.target.result);
                console.log("JSON loaded:", jsonData);
            } catch (err) {
                alert("Invalid JSON file");
            }
        };
        reader.readAsText(file);
    }

    const printAsPDF = () => {
        localStorage.setItem("cvData", JSON.stringify({ selectedCV, jsonData }));

        const win = window.open("/print", "_blank");

        win.onload = () => {
            win.focus();

            setTimeout(function () {
                win.print();
                win.close();
            }, 100);
        };
    };
</script>

<div class="max-w-md mx-auto mt-10 flex flex-col gap-4">
    <!-- File Input -->
    <label
        for="jsonFile"
        class="cursor-pointer bg-blue-700 hover:bg-blue-600 text-white font-semibold py-2 px-4 rounded text-center transition-colors"
    >
        Choose JSON file
    </label>
    <input id="jsonFile" type="file" accept=".json" class="hidden" onchange={handleFileChange} />
    {#if fileName}
        <div class="text-center text-gray-400 text-sm">Loaded file: {fileName}</div>
    {/if}

    <!-- CV Type Select -->
    <select
        bind:value={selectedCV}
        class="border border-gray-300 rounded px-3 py-2 bg-white text-gray-600 focus:outline-none focus:ring-2 focus:ring-blue-400 focus:border-blue-400"
    >
        {#each cvOptions as option}
            <option value={option}>{option.charAt(0).toUpperCase() + option.slice(1)}</option>
        {/each}
    </select>
</div>

{#if jsonData}
    <div class="w-full">
        <div
            class="max-w-xs mx-auto mt-2 p-4 bg-blue-700 hover:bg-blue-600 rounded overflow-x-auto text-center cursor-pointer"
            onclick={printAsPDF}
        >
            <button>Print as PDF</button>
        </div>
    </div>
{/if}
