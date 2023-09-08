<script lang="ts">

import anime from "animejs";
import { onMount } from "svelte";
import { homeAnchor, loadPagePromise, slickScrollInstance } from "../store";
import { loadImage } from "../utils";

// DOM Node Binds for animations
let homeContainer; // Container
let backgroundContainer, backgroundImage; // Offsets
let path1, path2, path3, path4; // SVG Paths
let titleWord1, titleWord2, shortDetails, callToAction; // Elements for animations

onMount(async () => {
	// Wait for page to load
	await loadPagePromise;
	// Set navbar home link's y location to top of homeContainer
	$homeAnchor = homeContainer;

	// Add parallax scrolling offsets to slickScroll
	$slickScrollInstance.addOffset({
		element: backgroundContainer,
		speedY: 0.8
	});

	introAnimations();
})


// Page load animations
function introAnimations() {

	let animation = [{ strokeDashoffset: '0' }];

	// Signature animation using svg strokDashOffset
	path1.animate(animation, {
		duration: 1000,
		delay: 500,
		easing: 'cubic-bezier(.72,.3,.25,1)',
		fill: 'forwards' 
	});
	path2.animate(animation, {
		duration: 300,
		delay: 1500,
		easing: 'cubic-bezier(.47,.41,.26,1)',
		fill: 'forwards' 
	});
	path3.animate(animation, {
		duration: 200,
		delay: 1800,
		easing: 'cubic-bezier(.47,.41,.26,1)',
		fill: 'forwards' 
	});
	path4.animate(animation, {
		duration: 1000,
		delay: 2000,
		easing: 'cubic-bezier(.47,.41,.26,1)',
		fill: 'forwards' 
	});


	// Animate background image
	Object.assign(backgroundContainer.style, {
		height: "0",
		transform: "scale(1.3)",
	});
	backgroundImage.style.transform = "translateY(80%) scale(1.4)";

	anime({
		targets: backgroundContainer,
		height: "100%",
		scale: 1,
		easing: "cubicBezier(0.165, 0.84, 0.44, 1)",
		duration: 1500,
		delay: 500,
		complete: () => {
			backgroundContainer.style.boxShadow = "3px 9px 18px rgba(0, 0, 0, 0.2)";
		}
	});
	anime({
		targets: backgroundImage,
		translateY: "0",
		scale: 1,
		easing: "cubicBezier(0.165, 0.84, 0.44, 1)",
		duration: 1500,
		delay: 500
	});


	// Animate title
	let titleElems = [titleWord1, titleWord2, shortDetails, callToAction];
	titleElems.forEach(e => {
		e.style.transform = "translateY(130%) rotate(10deg)";
	})
	anime({
		targets: titleElems,
		rotate: "0",
		translateY: "0%",
		easing: "cubicBezier(0.165, 0.84, 0.44, 1)",
		duration: 900,
		delay: anime.stagger(80, {start: 500})
	});
}

</script>



<div id="content-container" style="padding-top: 23vh" bind:this={homeContainer}>
	<div class="content-wrapper">
		<div class="flex">
			<div class="flex-wrapper first">

				<svg id="signature" class="h-signature" x="0px" y="0px" viewBox="0 0 190 136.9">
					<g>
						<path
							bind:this={path1}
							class="path-1"
							style="fill:none;stroke:#ffffff;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:miter;stroke-opacity:1;stroke-miterlimit:4;"
							d="M27 69.7038C27 69.7038 11 93.0001 42.5 86.4038C53.6 87.5038 44.303 3.30383 6.90299 2.00383C-11.597 1.40383 25.903 71.3038 58.603 86.4038C62.403 93.2038 56.403 57.4038 56.403 57.4038C56.403 57.4038 62.603 66.7038 64.303 63.5038C66.003 60.4038 64.403 58.4038 71.203 65.4038C72.203 66.6038 83.5 71.0001 90.003 66.7038C93.4399 61.1474 100.938 51.1425 107.5 48.2616M27 69.7038C19 71.8051 3.10002 78.7069 3.50002 89.5038C12 96.0001 21.5 88.5001 27 69.7038ZM138.5 54.7814C136.421 56.5204 134.304 58.167 132.5 59.5001C120.5 64.7002 110.833 54.1745 107.5 48.2616M138.5 54.7814C145.259 49.1284 151.618 42.5001 145.5 42.5001C135.5 42.5001 133 52.159 138.5 54.7814ZM138.5 54.7814C144.833 55.9568 155.5 59.7268 147.5 65.4038C137.5 72.5001 131 71.0001 129 70.5001C127 70.0001 132.5 59.5964 157.5 58.5001C177.5 57.6231 174.5 58.1347 170.5 58.5001M107.5 48.2616C108.891 47.6508 110.24 47.3602 111.5 47.5001C117.833 48.1667 130.346 44.9037 123.946 54.5037C115.946 66.5037 92.4464 74.5038 95.4464 63.5038C98.4464 52.5038 95 49.023 107.5 48.2616Z"/>
						</g>
				</svg>

			</div>
			
			<div class="flex-wrapper second">
				<h1 class = "title">
					<div class="title-mask">
						<div class="word" bind:this={titleWord1}>Almas</div>
					</div><br> 
					<div class="title-mask">
						<div class="word" bind:this={titleWord2}>Farhima</div>
					</div>
				</h1>
				<div class="occupation mask">
					<p class = "paragraph" bind:this={shortDetails}>
						Teacher & Calligraphy Artist, India
					</p>
				</div>
				<div class="wrapper action-mask">
					<div class="action" bind:this={callToAction}>
						<div class="mask">
							{#await loadImage("assets/imgs/scroll_arrow.png") then src}
								<img src="{src}" alt="">
							{/await}
						</div>
						<div>
							scroll
						</div>
					</div>
				</div>
			</div>

			<div class="parallax-wrapper home-back" bind:this={backgroundContainer}>
				{#await loadImage("assets/imgs/home-back.jpg") then src}
					<img src="{src}" bind:this={backgroundImage} draggable="false" alt="Home Background" style="width:100%; height: 100%; object-fit: cover;">
				{/await}
			</div>
		</div>
	</div>
</div>



<style lang="sass">

@import "../consts.sass"
@include textStyles()

#content-container
	height: 100vh
	width: 100vw
	padding: 12vh 7vw
	box-sizing: border-box
	position: relative

	.content-wrapper
		position: relative
		height: 100%
		box-sizing: border-box
		z-index: 2

	.flex
		z-index: 2
		width: 95%
		height: 100%
		display: flex
		flex-direction: row
		justify-content: space-between
		position: relative
		box-sizing: border-box

		.flex-wrapper
			position: relative
			height: 100%
			display: flex
			flex-direction: column
			justify-content: center

			&.second
				margin-right: 5vw 
				justify-content: flex-end

			h1
				font-weight: 400
				text-shadow: 0px 5px 10px rgba(0, 0, 0, 0.3)

			.title-mask
				overflow: hidden
				display: inline-flex

			.mask
				overflow: hidden

			.h-signature
				width: 35vh
				margin-left: -6vh

			.occupation
				position: relative
				margin-top: 8vh

			.action-mask
				margin-top: 10vh
				margin-right: 7vw
				display: inline-flex
				overflow: hidden

				.action
					font-size: 2vh
					letter-spacing: 0.5vh
					font-family: $font
					text-transform: uppercase
					color: white
					position: relative
					display: inline-flex
					flex-direction: row
					align-items: center

					.mask
						overflow: hidden
						height: 2vh

						img
							height: 2.3vh
							margin-right: 1.5vh
							animation: scrollArrowLoop 3s ease infinite

	.parallax-wrapper
		position: absolute
		left: 0
		z-index: -1
		width: 80%
		height: 100%
		margin-left: 5%
		border-radius: 1.5vh
		overflow: hidden
		box-sizing: border-box
		-webkit-touch-callout: none
		-webkit-user-select: none
		-moz-user-select: none
		-ms-user-select: none
		user-select: none
		transition: box-shadow 0.6s ease
		-webkit-transition: box-shadow 0.6s ease

		@media only screen and (max-width: 1250px)
			&
				opacity: 0.7

		@media only screen and (max-width: 750px)
			&
				opacity: 0.3

		img
			height: 100%
			width: 100%
			object-fit: cover
			border-radius: 1.5vh

@media only screen and (min-width: 1250px)
	.h-signature
		display: block

	.occupation
		width: 100%

	#content-container .flex *
		text-align: left

@media only screen and (max-width: 1250px)
	#content-container .flex *
		text-align: left

	.flex
		justify-content: center !important
		width: 100% !important

		.flex-wrapper 
			&.first
				display: none !important

			&.second
				justify-content: center !important
				margin: 0

	#content-container .flex .bottom
		text-align: left
		left: 5vw

	.parallax-wrapper
		width: 100% !important
		margin-left: 0 !important

@media only screen and (max-width: 750px)
	.occupation
		width: 70%


#signature
	.path-1
		stroke-dasharray: 365
		stroke-dashoffset: 365
	
	.path-2
		stroke-dasharray: 85
		stroke-dashoffset: 85

	.path-3
		stroke-dasharray: 45
		stroke-dashoffset: 45

	.path-4
		stroke-dasharray: 180
		stroke-dashoffset: 180


@keyframes scrollArrowLoop
	0%
		transform: translateY(-120%)
	
	30%
		transform: translateY(0%)
	
	70%
		transform: translateY(0%)
	
	100%
		transform: translateY(120%)

</style>