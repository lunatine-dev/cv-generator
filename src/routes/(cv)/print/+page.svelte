<script>
    import { onMount } from "svelte";
    import Title from "$lib/components/Title.svelte";
    import { keywords, highlight } from "$lib/tags.js";
    let stored = $state({});

    let type = $derived(stored?.selectedCV || "");
    let data = $derived(stored?.jsonData || {});

    onMount(() => {
        let localData = localStorage.getItem("cvData");

        stored = JSON.parse(localData);
    });

    let accentColor = "#008080";
</script>

<div class="text-sm">
    {#if Object.keys(data).length}
        <div class="mb-2">
            <Title type="sameLine" text={data.name} classes="text-2xl font-bold" />
            <div class="flex">
                {#if data?.role}
                    <Title text={data.role} />
                {/if}
            </div>
            <div class="flex items-center justify-between text-gray-700 text-sm gap-2">
                <span>{data.address}</span>
                <span>|</span>
                <span>{data.phone}</span>
                <span>|</span>
                <span>{data.email}</span>
            </div>
        </div>
        <Title type="newLine" text="Career Summary" classes="text-lg font-semibold uppercase" />
        <div class="mb-2 pl-2">
            {@html highlight(data.summary)}
        </div>
        <Title type="newLine" text="Skills" classes="text-lg font-semibold uppercase" />
        <div class="mb-2 pl-2">
            <ul class="list-disc list-outside ml-8 columns-3 gap-4">
                {#if data?.type === "general"}
                    {#each data.softskills as skill}
                        <li>{@html highlight(skill)}</li>
                    {/each}
                {:else}
                    {#each data.hardskills as skill}
                        <li>{@html highlight(skill)}</li>
                    {/each}
                {/if}
            </ul>
        </div>
        {#if data?.projects}
            <Title type="newLine" text="Projects" classes="text-lg font-semibold uppercase" />
            <div class="mb-2 pl-2">
                {#each data.projects as project}
                    <span class="font-semibold">{project.name}</span>
                    <span class="flex items-center justify-items-start text-gray-700">{project.type}</span>

                    <span class="flex">{project.description}</span>
                {/each}
            </div>
        {/if}
        <Title type="newLine" text="Work Experience" classes="text-lg font-semibold uppercase" />
        <div class="mb-2 pl-2">
            {#each data.experience as experience}
                <div class="mb-2">
                    <div class="flex items-center justify-between text-gray-700 text-sm gap-2">
                        <span class="font-semibold">{experience.title} </span>

                        <span> {experience.start} - {experience.end || "Present"}</span>
                    </div>
                    <span>{experience.name}</span>

                    {#if experience?.roles?.length}
                        <ul class="list-disc list-outside ml-8">
                            {#each experience.roles as role}
                                <li>{@html highlight(role)}</li>
                            {/each}
                        </ul>
                    {/if}
                </div>
            {/each}
        </div>
        <div class="education mt-5" style={type === "general" ? "" : "page-break-before: always;break-before: page;"}>
            <Title type="newLine" text="Education" classes="text-lg font-semibold uppercase" />
            <div class="mb-2 pl-2">
                {#each data.education as education}
                    <div class="mb-2">
                        <div class="flex items-center justify-between text-gray-700 text-sm gap-2 mb-1">
                            <span><span class="font-bold">{education.name}</span> | {education.description}</span>

                            <span>{education.start} - {education.end || "Present"}</span>
                        </div>

                        <ul class="list-disc list-outside ml-8">
                            {#each education.qualifications as qual}
                                <li>{@html highlight(qual)}</li>
                            {/each}
                        </ul>
                    </div>
                {/each}
            </div>
        </div>
    {/if}
</div>
