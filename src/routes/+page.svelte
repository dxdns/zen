<script lang="ts">
	import SoundWave from "@/components/sound-wave/index.js"
	import { ambientSoundsData } from "@/data/ambientSoundsData.js"
	import { Button, Card } from "@dxdns/feflow-svelte"

	let { data } = $props()

	let soundUrlPlaying: string | null = $state(null)
	let audio: HTMLAudioElement | null = $state(null)
	let isPlaying = $state(false)

	function playSound(url: string) {
		if (!audio) {
			audio = new Audio(url)
			audio.loop = true
			audio.play()
			soundUrlPlaying = url
			isPlaying = true
			return
		}

		if (soundUrlPlaying === url) {
			if (audio.paused) {
				audio.play()
				isPlaying = true
			} else {
				audio.pause()
				isPlaying = false
			}
		} else {
			audio.pause()
			audio.currentTime = 0
			audio.src = url
			audio.play()
			soundUrlPlaying = url
			isPlaying = true
		}
	}
</script>

<svelte:head>
	<title>Zen | {data.title}</title>
	<meta
		name="description"
		content="Relax with ambient zen sounds for meditation, stress relief, and deep calm. Perfect for mindfulness, yoga, and peaceful background atmospheres."
	/>
</svelte:head>

<div class="container">
	<header>
		<h1>Zen</h1>
	</header>

	<section id="sounds">
		<Card variant="contained">
			<h3 style="text-align: center;">
				Sounds ({ambientSoundsData.length}):
			</h3>
			<ul class="grid">
				{#each ambientSoundsData as { icon, label, key, url }, i (`${key}-${i}`)}
					<li>
						<Button
							style="width: 100%;"
							variant={soundUrlPlaying === url ? "contained" : "outlined"}
							onclick={() => playSound(url)}
						>
							<span style="font-size: 1.5rem;">{icon}</span>
							<span>{label}</span>
							{#if audio && soundUrlPlaying === url && isPlaying}
								<SoundWave />
							{/if}
						</Button>
					</li>
				{/each}
			</ul>
		</Card>
	</section>
</div>

<style>
	.container {
		width: 500px;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	@media screen and (max-width: 768px) {
		.container {
			width: 100%;
		}
	}

	.grid {
		all: unset;
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(200px, auto));
		gap: 1rem;
		align-items: center;
		justify-content: center;
	}

	.grid li {
		list-style: none;
	}

	@media screen and (max-width: 425px) {
		.grid {
			grid-template-columns: 1fr;
		}
	}
</style>
