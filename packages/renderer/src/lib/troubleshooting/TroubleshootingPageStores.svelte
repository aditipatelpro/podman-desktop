<script lang="ts">
import { onMount, onDestroy } from 'svelte';

import { type Unsubscriber } from 'svelte/store';
import type { EventStoreInfo } from '/@/stores/event-store';
import { allEventStoresInfo } from '/@/stores/event-store-manager';
import TroubleshootingPageStore from './TroubleshootingPageStore.svelte';
import Fa from 'svelte-fa';
import { faDatabase } from '@fortawesome/free-solid-svg-icons';

let allEventstores: EventStoreInfo[] = [];

let allEventsUnsubscriber: Unsubscriber;

onMount(() => {
  allEventsUnsubscriber = allEventStoresInfo.subscribe(value => {
    // sort the store
    value.sort((a, b) => a.name.localeCompare(b.name));
    allEventstores = value;
  });
});

onDestroy(() => {
  allEventsUnsubscriber?.();
});
</script>

<div class="flex flex-col bg-charcoal-600 p-4 rounded-lg">
  <div class="flex flex-row align-middle items-center w-full mb-4">
    <Fa size="30" class="pr-3 text-gray-700" icon="{faDatabase}" />
    <div role="status" aria-label="stores" class="text-xl">Stores</div>
  </div>

  <div class="grid grid-cols-2 lg:grid-cols-4 gap-x-4 gap-y-4">
    {#each allEventstores as eventStore (eventStore.name)}
      <TroubleshootingPageStore eventStoreInfo="{eventStore}" />
    {/each}
  </div>
</div>
