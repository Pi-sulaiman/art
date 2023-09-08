<script lang="ts">

import { onMount } from "svelte";
import { letterSlideIn, maskSlideIn } from "../animations";
import { dataFetch } from "../store"

let footerContainer;
let logoElem, creditsElem, statusElem, fullEmailLinkElem;
let signaturePath1, signaturePath2, signaturePath3, signaturePath4; 

let currentYear = new Date().getFullYear();

onMount(() => {
    introAnimations();
});

async function introAnimations() {
    // Scroll activated animations powered by anime instead of svelte transitions
	const logoAnimate = maskSlideIn(logoElem, {});
    const fullEmailLinkAnimate = letterSlideIn(fullEmailLinkElem, { delay: 6, initDelay: 150 });
    const creditsAnimate = maskSlideIn(creditsElem, { delay: 150 });

    // Intersection observer to run animations when footer is in scroll view
    let animationObserver = new IntersectionObserver((entries) => { 
        entries.forEach(entry => {
            if (entry.isIntersecting) {

				logoAnimate.anime();
                creditsAnimate.anime();
                statusAnimate.anime();
				fullEmailLinkAnimate.anime();

                // Signature SVG animation
                let animation = [{ strokeDashoffset: '0' }];

                // Signature animation using svg strokDashOffset
                signaturePath1.animate(animation, {
                    duration: 1000,
                    delay: 0,
                    easing: 'cubic-bezier(.72,.3,.25,1)',
                    fill: 'forwards' 
                });
                signaturePath2.animate(animation, {
                    duration: 300,
                    delay: 1000,
                    easing: 'cubic-bezier(.47,.41,.26,1)',
                    fill: 'forwards' 
                });
                signaturePath3.animate(animation, {
                    duration: 200,
                    delay: 1300,
                    easing: 'cubic-bezier(.47,.41,.26,1)',
                    fill: 'forwards' 
                });
                signaturePath4.animate(animation, {
                    duration: 1000,
                    delay: 1500,
                    easing: 'cubic-bezier(.47,.41,.26,1)',
                    fill: 'forwards' 
                });

                animationObserver.disconnect();
            }
        });
    }, {
        root: null,
        threshold: 0.4
    });

    animationObserver.observe(footerContainer);

    await dataFetch;
	const statusAnimate = letterSlideIn(statusElem, { delay: 6, initDelay: 100 });
}

</script>



<div class="footer-wrapper" bind:this={footerContainer}>
    <!-- Left side -->
    <div class="flex-wrapper">
        <div class="logo-wrapper">
            <div class="inline-flex" bind:this={logoElem}>
                <img src="assets/imgs/logo.svg" alt="mh logo" class="logo">
            </div>
        </div>

        <div class="status-wrapper">
            {#await dataFetch then fetchedData}
                {#if fetchedData.availablity_date == ""}
                    <p class="large-text" bind:this={statusElem}>
                        i am currently accepting freelance work, <br>you may reach me on my email.
                    </p>
                {:else}
                    <p class="large-text" bind:this={statusElem}>
                        <!-- i am available for freelance work after <br> {fetchedData.availablity_date}. -->
                        I'm Almas Fathima, a teacher and a calligraphy artist, <br>  bridging education and artistry.
                    </p>
                {/if}
            {/await}
            <a class="button large-text"  href="https://www.instagram.com/arshii_almas_24/" target="_blank">instagram</a>
        </div>
        
        <div class="credits-wrapper" bind:this={creditsElem}>
            <p class="year">© {currentYear}</p>
            <p class="credits">
                D   esigned and developed by Sulaiman<br>Hi There
            </p>
        </div>
    </div>

    <!-- Right side -->
	<div class="flex-wrapper decor">
        <!-- Musab Hassan SVG Signature -->
        <svg id="signature" class="name-signature" x="0px" y="0px" viewBox="0 0 190 136.9" style="stroke: rgb(79, 78, 85);">
            <g>
                <path
                    bind:this={signaturePath1}
                    class="path-1"
                    style="fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:miter;stroke-opacity:1;stroke-miterlimit:4;"
                    d="M27 69.7038C27 69.7038 11 93.0001 42.5 86.4038C53.6 87.5038 44.303 3.30383 6.90299 2.00383C-11.597 1.40383 25.903 71.3038 58.603 86.4038C62.403 93.2038 56.403 57.4038 56.403 57.4038C56.403 57.4038 62.603 66.7038 64.303 63.5038C66.003 60.4038 64.403 58.4038 71.203 65.4038C72.203 66.6038 83.5 71.0001 90.003 66.7038C94.1687 59.9693 104.3 46.7001 111.5 47.5001C114.833 47.7014 120.9 49.9638 118.5 57.4038C115.5 66.7038 114 75.9038 105 69.7038C96 63.5038 93 48.5 101 49.5C107.4 50.3 114.096 55.8372 116.596 63.5038C119.096 71.1705 132.5 59.5001 132.5 59.5001C134.304 58.167 136.421 56.5204 138.5 54.7814M27 69.7038C19 71.8051 3.10002 78.7069 3.50002 89.5038C12 96.0001 21.5 88.5001 27 69.7038ZM138.5 54.7814C145.259 49.1284 151.618 42.5001 145.5 42.5001C135.5 42.5001 133 52.159 138.5 54.7814ZM138.5 54.7814C144.833 55.9568 155.5 59.7268 147.5 65.4038C137.5 72.5001 131 71.0001 129 70.5001C127 70.0001 132.5 59.5964 157.5 58.5001C177.5 57.6231 174.5 58.1347 170.5 58.5001"/>
                <!-- <path
                    bind:this={signaturePath2}
                    class="path-2"
                    style="fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:miter;stroke-opacity:1;stroke-miterlimit:4;"
                    d="M132.2,48.3l-23.9,78.8"/>
                <path
                    bind:this={signaturePath3}
                    class="path-3"
                    style="fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:miter;stroke-opacity:1;stroke-miterlimit:4;"
                    d="M110.3,55.3c0,0-0.7,11.7-2.8,18s-6.7,20.2-6.9,24.1"/>
                <path
                    bind:this={signaturePath4}
                    class="path-4"
                    style="fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:miter;stroke-opacity:1;stroke-miterlimit:4;"
                    d="M122,74.4c0,0-5.9-8-17.1-6.7c-11.1,1.3-20.2,11.3-21.1,12.6c-0.9,1.3-10,9.6,2.2,15s38.9-7.2,38.9-7.2s17.8-10,18.9-10s-4.6,5.9-4.3,7.2c0.4,1.3,2.8,2,7.2-1.5c1-0.8,17.2-0.8,22.2,1c1.9,0.7,3.5-0.2,5-1.4c1-0.8,9.4,2,9.4,2"/> -->
            </g>
        </svg>
    </div>
</div>



<style lang="sass">

@import "../consts.sass"
@include textStyles()

.footer-wrapper
    width: 100vw
    background-color: #131314
    display: flex
    flex-direction: row
    justify-content: space-between
    padding: 15vh 13vw
    margin-top: 25vh
    box-sizing: border-box

    @media only screen and (max-width: 950px)
        .flex-wrapper.decor
            display: none !important

    @media only screen and (max-width: 950px)
        flex-direction: column-reverse

        .flex-wrapper:not(:first-child)
            margin-bottom: 15vh

    .inline-flex
        flex-grow: 1
        display: flex
        flex-direction: row
        align-items: center


    .logo-wrapper
        margin-bottom: 5vh

        .logo
            display: inline-block
            height: 6vh

    .status-wrapper
        .button.large-text
            margin-top: 2vh

    .credits-wrapper
        margin-top: 5vh
        color: rgba(255,255,255,0.3)

        p.year
            margin-bottom: 1vh
            font-family: $font
            font-size: 1.8vh
            font-weight: normal
            color: rgba(255,255,255,0.3)

        .credits
            font-size: 1.5vh
            line-height: 125%
            white-space: nowrap
            color: rgba(255,255,255,0.3)

    .large-text
        font-size: 2.5vh

        @media only screen and (max-width: 950px)
            br
                display: none

    .flex-wrapper.decor
        display: flex
        flex-direction: column
        justify-content: center

        .name-signature
            width: 20vh

#signature
    .path-1
        stroke-dasharray: 1400
        stroke-dashoffset: 100
    


</style>