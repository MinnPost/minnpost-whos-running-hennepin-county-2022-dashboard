<script>
    // all data for candidates and races
    export let items;

    // the candidates from App.svelte
	let candidates = items.candidates;

    // create a list of candidates for an office
	let office_candidates = function(office) {
        return items.candidates.filter(
            item => item["race-id"] == office
        );
	}

    // single candidate template
	import Candidate from "./Candidate.svelte";

    // link to go back to unfiltered list
    import LinkToFullCandidateList from "./components/LinkToFullCandidateList.svelte";
    
</script>

{#if items["searchTerm"] != ""}
    <aside class="m-search-result-info">
        Showing {items.candidates.length} {#if items.candidates.length == 1}candidate{:else}candidates{/if} in {items.races.length} {#if items.races.length == 1}race{:else}races{/if} for <strong>{items["searchTerm"]}</strong>.
    </aside>
{/if}

{#each items.races as race}
    <section class="race-listing">
        <h2 class="m-archive-header">{race.office}</h2>
        {#if race.blurb}
            <p>{@html race.blurb}</p>
        {/if}
        {#if office_candidates(race["office-id"]).length > 0}
            <section class="m-archive m-archive-homepage m-zone m-zone-homepage-more-top candidates-list">
                {#each office_candidates(race["office-id"]) as candidate}
                    <Candidate candidate = {candidate} />
                {/each}
            </section>
        {/if}
    </section>
{/each}
