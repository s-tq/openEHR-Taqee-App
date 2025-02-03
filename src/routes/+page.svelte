<script lang="ts">
    import "medblocks-ui";
    import "medblocks-ui/dist/styles.js";
    import { tick } from 'svelte';
    import webtemplate from './vitalsigns_taqee_v0.2.json';

    let ehrId = "b49cb360-3ea2-4e44-bd14-4a1189d0dba7";
    let openEhrRestEndpoint = "https://openehr-bootcamp.medblocks.com/ehrbase/rest/openehr/v1";
    let form: HTMLElement | null = null;  // Ensure correct typing

    async function submitVitals() {
        await tick();  // Ensures form is initialized

        if (!form) {
            console.error("🚨 ERROR: `form` is still undefined after tick().");
            return;
        }

        try {
            const composition = form.export();
            console.log("✅ Exported composition:", composition);

            let response = await fetch(`${openEhrRestEndpoint}/ehr/${ehrId}/composition?templateID=${webtemplate.templateId}`, {
                method: "POST",
                body: JSON.stringify(composition),
                headers: {
                    "Content-Type": "application/openehr.wt.flat.schema+json",
                    "Accept": "application/openehr.wt.flat.schema+json",
                    "Prefer": "return=representation"
                }
            });

            let data = await response.json();
            console.log("✅ Response Data:", data);
        } catch (error) {
            console.error("🚨 ERROR in `submitVitals`:", error);
        }
    }
</script>

<div class="flex w-full gap-8 p-4">
    <!-- Left Side: Form -->
    <div class="flex-1 bg-white shadow-lg rounded-lg p-6">
        <h2 class="text-2xl font-bold mb-6 text-gray-800">Vital Sign Assessment</h2>
        <mb-auto-form bind:this={form} webTemplate={webtemplate} hideSubmitButton></mb-auto-form>

        <div class="mt-4 flex justify-end w-full">
            <button on:click={submitVitals} class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded transition-all">
                Submit Vitals
            </button>
        </div>
    </div>

    <!-- Right Side: Previous Records -->
    <div class="flex-1 bg-gray-50 shadow-md rounded-lg p-6">
        <h2 class="text-xl font-semibold mb-4">Previous Records</h2>
        <!-- Previous records content will go here -->
    </div>
</div>
