<script>
	import { onMount } from 'svelte';
	import * as THREE from 'three';
	let group;

	let container;

	let camera, scene, renderer;

	let mouseX = 0,
		mouseY = 0;

	let width = window.innerWidth;
	let height = window.innerHeight;

	let sphere, sphere_2;

	let windowHalfX = width / 2;
	let windowHalfY = height / 2;

	init();
	animate();

	function init() {
		camera = new THREE.PerspectiveCamera(20, width / height, 1, 10000);
		camera.position.z = 450;

		scene = new THREE.Scene();
		scene.background = new THREE.Color(0xf4f4f4);

		const light = new THREE.DirectionalLight(0xf4f4f4);
		light.position.set(0, 1, 1);
		scene.add(light);

		// -------------------------------------------------------------------------

		sphere = new THREE.Mesh(
			new THREE.SphereGeometry(170, 240, 120),
			new THREE.MeshBasicMaterial({ color: 0xf4f4f4, wireframe: false })
		);

		sphere_2 = new THREE.Mesh(
			new THREE.SphereGeometry(171, 480, 240),
			new THREE.MeshBasicMaterial({ color: 0x2b2b2b, wireframe: true })
		);

		scene.add(sphere, sphere_2);

		scene.rotation.x += Math.PI / 2;

		// -------------------------------------------------------------------------

		renderer = new THREE.WebGLRenderer({ antialias: true });
		renderer.setPixelRatio(window.devicePixelRatio);
		renderer.setSize(width, height);

		onMount(() => {
			container.appendChild(renderer.domElement);
		});

		document.addEventListener('mousemove', onDocumentMouseMove);

		//

		window.addEventListener('resize', onWindowResize);
	}

	function onWindowResize() {
		let width = window.innerWidth;
		let height = window.innerHeight;

		windowHalfX = width / 2;
		windowHalfY = height / 2;

		camera.aspect = width / height;
		camera.updateProjectionMatrix();

		renderer.setSize(width, height);
	}

	function onDocumentMouseMove(event) {
		mouseX = event.clientX;
		mouseY = event.clientY;
	}

	function animate() {
		requestAnimationFrame(animate);
		render();
	}

	function render() {
		camera.position.z = (mouseY + mouseX) * 0.1;
		scene.rotation.y = mouseX / 100000 + scene.rotation.y;

		camera.lookAt(scene.position);

		renderer.render(scene, camera);
	}
</script>

<div bind:this={container} class:geometry={true} />

<style>
	.geometry {
		overflow: hidden;
		opacity: 0.9;
	}
</style>
