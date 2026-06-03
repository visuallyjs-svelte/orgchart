<script lang="ts">
    import { type SvelteWrapperProps, useZoom } from "@visuallyjs/browser-ui-svelte"
    import officeLocations from "../office-locations"

    const {
        data, model, vertex, ui
    }:SvelteWrapperProps = $props()

    const zoom = useZoom(ui)

    function getImage() {
        return `/avatars/${data.img}`
    }

    const timezone = $derived.by(() => {
        const locationData = officeLocations.find(loc => loc.name === data.location)
        return locationData ? locationData.timezone : ""
    })

    function getTimezoneOffset(tz: string) {
        return tz.match(/\((UTC[+-]\d+)\)/)?.[1] || tz;
    }
</script>

<img src={getImage()} alt={data.name}/>
<div>
    <strong>{data.name}</strong>
    <span class="vjs-node-title">{data.title}</span>
    {#if zoom.current > 1}
        <div class="vjs-node-status-container">
            <span class="vjs-node-status {data.online ? 'vjs-node-status-online' : 'vjs-node-status-offline'}"></span>
            <span class="vjs-node-status-text">{data.online ? 'Online' : 'Offline'}</span>
        </div>
        <a href="mailto:{data.email}" class="vjs-node-email">{data.email}</a>
        <span class="vjs-node-location">
            {data.location}
            {#if timezone}
                <span class="vjs-node-timezone">({getTimezoneOffset(timezone)})</span>
            {/if}
        </span>
    {/if}
</div>
