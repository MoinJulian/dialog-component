<script lang="ts">
	import Dialog, { open_dialog } from "./lib/Dialog.svelte"
	import Header from "./lib/Header.svelte"
	import Items from "./lib/Items.svelte"
	import Menu from "./lib/Menu.svelte"
	import { type Item } from "./lib/types"
	import { shuffle } from "./lib/utils"

	const AMOUNT = 2

	const original_items = Array.from({ length: AMOUNT }).map((_, i) => ({
		source: `https://unsplash.it/${300 + i}/200`,
		id: crypto.randomUUID(),
	}))

	let items: Item[] = original_items

	function confirm_delete_item(item: Item) {
		open_dialog({
			text: "Do you really want to delete the item?",
			modal: true,
			confirm: {
				text: "Yes",
				action: () => delete_item(item),
			}
		})
	}

	function delete_item(item: Item) {
		items = items.filter((_item) => _item != item)
		if (items.length === 0) setTimeout(confirm_create_items, 500)
	}

	function open_shuffle_modal() {
		open_dialog({
			text: "This will shuffle the items in a random order.",
			modal: true,
			confirm: {
				text: "Ok", action: shuffle_items
			},
			cancel: {
				text: "Cancel",
				action: () => {},
			}
		})
	}

	function shuffle_items() {
		items = shuffle(items)
	}

	function confirm_create_items() {
		open_dialog({
			text: "There are no items left. Do you want to create new items?",
			confirm: {
				text: "Sure", action: () => items = original_items
			},
			cancel: {
				text: "No thanks", action: () => {}
			},
			modal: false
		})
	}

	function alert_rotate() {
		open_dialog({
			text: "This feature is not implemented yet",
			modal: false,
			cancel: null,
		})
	}
</script>

<Header />

<main>
	<Menu {open_shuffle_modal} {alert_rotate} />

	<Items {items} {confirm_delete_item} />
</main>

<Dialog></Dialog>