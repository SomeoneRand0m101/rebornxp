<script>
    import * as utils from '../../lib/utils';
    import { onMount, createEventDispatcher } from 'svelte';
    import { set, get } from 'idb-keyval';
    import { hardDrive, wallpaper } from '../../lib/store';
    import { bliss_wallpaper } from '../../lib/system';

    let dispatcher = createEventDispatcher();

    onMount(async () => {
        await load_hard_drive();
        await load_wallpaper();
        dispatcher('load_page', { url: './xp/desktop.svelte' }); // Directly load the desktop
    });

    async function load_hard_drive() {
        let drive = await get('hard_drive');
        if (!drive) {
            drive = (await fetch('/json/hard_drive.json')).json();
            await set('hard_drive', drive);
        }
        hardDrive.set(drive);
    }

    async function load_wallpaper() {
        let currentWallpaper = await get('wallpaper');
        if (!currentWallpaper) {
            currentWallpaper = bliss_wallpaper;
            await set('wallpaper', bliss_wallpaper);
        }
        wallpaper.set(currentWallpaper);
    }
</script>

<style>
    /* Remove all styles related to the boot screen */
</style>
