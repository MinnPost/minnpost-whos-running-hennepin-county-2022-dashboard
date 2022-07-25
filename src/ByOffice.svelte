<script>
	// all data for candidates and races
    export let items;

	// page.js data, such as the office we want
	export let params;

	// the races from App.svelte
	let races = items.races;

	// what office do we want?
	let office = {};
    if (params && params.office) {
		office = items.prefilteredRaces.find(item => item["office-id"] === params["office"]);
		races = [office];
    } else {
        // the distinct office names from the candidates
	    races = items.races;
    }

	// create a list of candidates for an office
	let office_candidates = function(office) {
        return items.candidates.filter(
            item => item["race-id"] == office
        );
	}

	// single candidate template
    import Candidate from "./Candidate.svelte";

	// link to go back to unfiltered list
    //import LinkToFullCandidateList from "./components/LinkToFullCandidateList.svelte";

</script>

{#if office}
	<aside class="m-search-result-info">
		{#if items["searchTerm"] != ""}
			Showing {office_candidates(office["office-id"]).length} {#if office_candidates(office["office-id"]).length == 1}search result{:else}search results{/if} for <strong>{items.searchTerm}</strong> within all {office["office"]} candidates.
		{:else}
			Showing {office_candidates(office["office-id"]).length} <strong>{office["office"]}</strong> {#if office_candidates(office["office-id"]).length == 1}candidate{:else}candidates{/if}.
		{/if}
	</aside>
{/if}

{#each races as race, key}
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
