<script lang="ts">
	import { SurfaceProvider, SurfaceComponent, ControlsComponent, MiniviewComponent} from "@visuallyjs/browser-ui-svelte"

    import renderOptions from "./render-options"
    import createViewOptions from "./view-options"
    import InspectorComponent from "./components/InspectorComponent.svelte"

    let surfaceComponent:SurfaceComponent

    // set a person to be the current selection. We also zoom to focus on that individual.
    function selectPerson(obj: any) {
        surfaceComponent.getModel().setSelection(obj)
        surfaceComponent.getSurface().centerOnAndZoom(obj, 0.15)
    }

    const viewOptions = createViewOptions(selectPerson)

</script>

<SurfaceProvider>
    <SurfaceComponent bind:this={surfaceComponent} className="vjs-orgchart-canvas" {renderOptions} {viewOptions} url="/dataset.json">
        <ControlsComponent undoRedo={false} clear={false}/>
        <MiniviewComponent/>
    </SurfaceComponent>
    <div class="vjs-orgchart-rhs">
        <InspectorComponent onSelect={selectPerson}/>
    </div>
</SurfaceProvider>
