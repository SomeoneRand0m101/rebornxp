<script>
    import { onMount, createEventDispatcher } from 'svelte';
    import * as utils from '../lib/utils';
    let dispatcher = createEventDispatcher();

    let is_chromium = true;

    onMount(() => {
        loadjs([
            'https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.1/jquery.min.js',
            'https://cdnjs.cloudflare.com/ajax/libs/jqueryui/1.13.2/jquery-ui.min.js',
            'https://code.jquery.com/ui/1.13.2/themes/base/jquery-ui.css'
        ], { async: false });

        utils.set_theme('none');
        is_chromium = window.chrome != null;

        // Automatically boot after onMount
        boot();
    });

    let current_option = 0; // Default to "Start Windows Normally"
    let boot_options = [
        'Start Windows Normally',
        'Install Windows',
    ];

    let selected = false;

    function boot() {
        if (selected) return;
        selected = true;

        if (current_option === 0) {
            utils.set_installing_windows(false);
            dispatcher('load_page', { url: './xp/starting.svelte' });
        } else if (current_option === 1) {
            utils.set_installing_windows(true);
            dispatcher('load_page', { url: './installation/dos/starting.svelte' });
        }
    }
</script>

<div class="w-screen h-screen bg-black overflow-hidden font-MSSS flex flex-col">
    <div class="mt-12 ml-8 text-lg flex-grow">
        <p class="text-slate-100">Booting automatically...</p>
    </div>
</div>
