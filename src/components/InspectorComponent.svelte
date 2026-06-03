<script lang="ts">
    import { InspectorComponent } from "@visuallyjs/browser-ui-svelte";
    import officeLocations from "../office-locations";

    let { onSelect } = $props();

    let current = $state(null);
    let manager = $state(null);
    let reports = $state([]);

    const timezone = $derived.by(() => {
        if (!current) return "";
        const locationData = officeLocations.find(loc => loc.name === current.data.location)
        return locationData ? locationData.timezone : ""
    })

    function getTimezoneOffset(tz: string) {
        return tz.match(/\((UTC[+-]\d+)\)/)?.[1] || tz;
    }

    function getImage(person: any) {
        return `/avatars/${person.data.img}`;
    }

    // invoked by the InspectorComponent when nothing is selected. We clear all our state.
    const renderEmptyContainer = () => {
        current = null;
        manager = null;
        reports = [];
    };

    // Invoked by the InspectorComponent when something is selected. We update our state.
    const refresh = (obj: any) => {
        current = obj;
        manager = obj.getTargetEdges().map((e: any) => e.source)[0];
        reports = obj.getSourceEdges().map((e: any) => e.target);
    };

    function selectPerson(person: any, e: MouseEvent) {
        e.preventDefault();
        if (onSelect) {
            onSelect(person);
        }
    }
</script>

<InspectorComponent {refresh} {renderEmptyContainer}>
    {#if current != null}
        <div class="vjs-orgchart-inspector">
            <h1>{current.data.name}</h1>
            <h2>{current.data.title}</h2>

            <div class="vjs-orgchart-inspector-details">
                <div class="vjs-node-status-container">
                    <span class="vjs-node-status {current.data.online ? 'vjs-node-status-online' : 'vjs-node-status-offline'}"></span>
                    <span class="vjs-node-status-text">{current.data.online ? 'Online' : 'Offline'}</span>
                </div>
                <a href="mailto:{current.data.email}" class="vjs-node-email">{current.data.email}</a>
                <span class="vjs-node-location">
                    {current.data.location}
                    {#if timezone}
                        <span class="vjs-node-timezone"> ({getTimezoneOffset(timezone)})</span>
                    {/if}
                </span>
            </div>

            {#if manager != null}
                <h5>Reports to:</h5>
                <a class="vjs-orgchart-inspector-person" href="#" data-id={manager.data.id} onclick={(e) => selectPerson(manager, e)}>
                    <img src={getImage(manager)} alt={manager.data.name} />
                    <div>
                        {manager.data.name}
                        <span>{manager.data.title}</span>
                    </div>
                </a>
            {/if}

            {#if reports.length > 0}
                <br />
                <h5>Reports:</h5>
                {#each reports as r (r.data.id)}
                    <a class="vjs-orgchart-inspector-person" href="#" data-id={r.data.id} onclick={(e) => selectPerson(r, e)}>
                        <img src={getImage(r)} alt={r.data.name} />
                        <div>
                            {r.data.name}
                            <span>{r.data.title}</span>
                        </div>
                    </a>
                {/each}
            {/if}
        </div>
    {/if}
</InspectorComponent>
