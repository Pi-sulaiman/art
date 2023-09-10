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
		duration: 800,
		delay: 500,
		easing: 'cubic-bezier(.72,.3,.25,1)',
		fill: 'forwards' 
	});
	path2.animate(animation, {
		duration: 1000,
		delay: 1300,
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
							d="M22.5868 103.388C26.2338 89.2127 35.8398 59.0872 45.088 51.986C47.8129 58.5092 44.2966 85.9441 42.1979 98.8462C42.1635 101.736 44.0145 107.104 51.6938 105.452C59.3731 103.801 143.247 87.4924 157.387 84.8087C164.311 83.4948 167.165 70.4609 159.142 66.1266C145.261 58.6279 162.5 43.0166 162.5 61"/>
					
						<path
							bind:this={path2}
							class="path-2"
							style="fill:none;stroke:#ffffff;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:miter;stroke-opacity:1;stroke-miterlimit:4;"
							d="M12 91.6993C12 91.6993 48.0442 88.7182 58.3426 81.9429C68.6409 75.1677 86.7985 55.1132 77.0422 49.422C67.2859 43.7308 43.7081 97.3905 59.1556 103.082C66.4728 105.777 80.8363 79.504 84.3594 64.8695L75.4161 99.0167C81.1073 87.0923 93.9532 63.2435 99.807 63.2435C105.661 63.2435 95.7418 83.8402 90.0506 94.1385C96.2838 83.5691 109.888 62.9183 114.441 64.8695C120.133 67.3086 103.059 90.0734 109.563 94.1385C114.767 97.3906 128.682 91.6783 134.767 82.7561C139.328 76.0686 147.776 60.8044 138.832 62.4304C129.889 64.8695 116.068 78.691 123.385 88.4473C126.983 93.2446 136.122 77.878 140.458 71.3737C139.916 79.504 141.109 93.1629 150.215 82.7561C159.321 72.3494 162.139 63.7855 162.41 60.8044"/>
					
					
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
		#signature
			position: absolute
			bottom: -20px
			left: 26vw
			width: 23vh
			margin-left: 0vh
			transform: rotate(342deg)
	.flex
		justify-content: center !important
		width: 100% !important

		.flex-wrapper 
			&.first
				//display: none !important
				

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
		stroke-dashoffset: 314
		stroke-dasharray: 314

	
	.path-2
		stroke-dasharray: 550
		stroke-dashoffset: 550

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
