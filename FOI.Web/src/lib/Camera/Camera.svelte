<script lang="ts">
	let videoElement: HTMLVideoElement;

	const constraint: MediaStreamConstraints = {
		video: { facingMode: 'environment' }
	};

	const streamPromise = async () =>
		navigator.mediaDevices.getUserMedia(constraint).then((stream) => {
			videoElement.srcObject = stream;
			videoElement.play();
			return stream;
		});

	function takePhoto() {
		const canvas = document.createElement('canvas');
		var context = canvas.getContext('2d');
		context.drawImage(videoElement, 0, 0, 640, 480);
		document.getElementsByClassName("camera")[0].appendChild(canvas);
	}
</script>

<div class="camera">
	{#await streamPromise()}
		<p>... requesting camera permissions</p>
	{:catch error}
		{#if error.name === 'NotAllowedError'}
			<p>Camera permissions were denied but are required to take a photo</p>
			<p>You can reload the page to allow camera permissions</p>
		{:else if error.name === 'NotReadableError'}
			<p>Camera was not available</p>
			<p>You can close other applications that are using the camera and reload the page to try again</p>
		{:else}
			<p>... camera permissions error</p>
			<p style="color: red">{error}</p>
		{/if}
	{/await}

	<!-- svelte-ignore a11y-media-has-caption -->
	<video bind:this={videoElement} />
	<button class="button-snap" on:click="{takePhoto}"/>
</div>

<style>
	.camera{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: stretch;
		flex: 1;
	}
	video{
		width: 100%;
		height: 100%;
		object-fit: cover;
	}
	.button-snap {
		position: absolute;
		width: 100px;
		height: 100px;
		top: 50%;
		left: 50%;
		transform:translate(-50%, -50%);

		border-radius: 50%;
		opacity: 0.6;
		border: 3px solid #000;
		background-color: #fff;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
		cursor: pointer;
		z-index: 1;
	}
</style>
