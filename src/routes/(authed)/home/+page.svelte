<script lang="ts">
	import ClipList from '$lib/view/clip-list.svelte';
	import ClipAdd from '$lib/controls/clip-add.svelte';
	import ClipModal from '$lib/view/clip-modal.svelte';
	import { crossfade, fade } from 'svelte/transition';

	let unfocus: boolean;
	let clipModal: boolean;

	const onCreate = () => {
		clipModal = true;
	};

	const onClose = () => {
		clipModal = false;
	};

	const [send, receive] = crossfade({
		duration: (d) => Math.sqrt(d * 200)
	});

	$: {
		if (clipModal) {
			unfocus = true;
		} else {
			setTimeout(() => {
				unfocus = false;
			}, 400);
		}
	}
</script>

<div class="root">
	<div class="head">
		{#if !clipModal}
			<div
				class="add-wrapper"
				out:send={{ key: 'clipflip', duration: 400 }}
				in:receive={{ key: 'clipflip', duration: 400 }}
			>
				<ClipAdd on:create={onCreate} />
			</div>
		{/if}
	</div>

	<div class="content">
		<ClipList unfocused={unfocus} />
	</div>
</div>

{#if clipModal}
	<div class="overlay" in:fade={{ duration: 200 }} out:fade={{ duration: 200 }}>
		<ClipModal {receive} {send} on:close={onClose} />
	</div>
{/if}

<style lang="scss">
	.root {
		.head {
			height: 50px;
		}

		.content {
			margin-top: 16px;
		}
	}

	.overlay {
		z-index: 10;

		position: absolute;
		left: 0;
		top: 0;

		width: 100%;
		height: 100%;

		backdrop-filter: blur(6px);
		background-color: rgba(var(--color-primary-rgb), 0.3);
	}
</style>
