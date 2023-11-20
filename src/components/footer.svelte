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
                        duration: 700,
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
                            i am currently working, <br>you may reach me on my instagram.
                        </p>
                    {:else}
                        <p class="large-text" bind:this={statusElem}>
                             <!-- i am available for freelance work after <br> {fetchedData.availablity_date}. -->
                        I'm Almas Fathima, a teacher and a calligraphy artist, <br>  bridging education and artistry.
                        </p>
                    {/if}
                {/await}
                <a class="button large-text" bind:this={fullEmailLinkElem} href="https://www.instagram.com/arshii_almas_24/" target="_blank">instagram</a>
            </div>
            
            <div class="credits-wrapper" bind:this={creditsElem}>
                <p class="year">© {currentYear}</p>
                <p class="credits">
                    Designed and developed by Sulaiman<br>Hi There
                </p>
                <img style="display: none;" src="https://moe-counter.glitch.me/get/@almas" alt="">
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
                        d="M12 91.6993C12 91.6993 48.0442 88.7182 58.3426 81.9429C68.6409 75.1677 86.7985 55.1132 77.0422 49.422C67.2859 43.7308 43.7081 97.3905 59.1556 103.082C66.4728 105.777 80.8363 79.504 84.3594 64.8695L75.4161 99.0167C81.1073 87.0923 93.9532 63.2435 99.807 63.2435C105.661 63.2435 95.7418 83.8402 90.0506 94.1385C96.2838 83.5691 109.888 62.9183 114.441 64.8695C120.133 67.3086 103.059 90.0734 109.563 94.1385C114.767 97.3906 128.682 91.6783 134.767 82.7561C139.328 76.0686 147.776 60.8044 138.832 62.4304C129.889 64.8695 116.068 78.691 123.385 88.4473C126.983 93.2446 136.122 77.878 140.458 71.3737C139.916 79.504 141.109 93.1629 150.215 82.7561C159.321 72.3494 162.139 63.7855 162.41 60.8044"/>
                    <path
                        bind:this={signaturePath2}
                        class="path-2"
                        style="fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:miter;stroke-opacity:1;stroke-miterlimit:4;"
                        d="M22.5868 103.388C26.2338 89.2127 35.8398 59.0872 45.088 51.986C47.8129 58.5092 44.2966 85.9441 42.1979 98.8462C42.1635 101.736 44.0145 107.104 51.6938 105.452C59.3731 103.801 143.247 87.4924 157.387 84.8087C164.311 83.4948 167.165 70.4609 159.142 66.1266C145.261 58.6279 162.5 43.0166 162.5 61"/>
                    
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
        position: absolute
        z-index: 99999
    
        @media only screen and (max-width: 950px)
            .flex-wrapper.decor
               //    display: none !important
    
        @media only screen and (max-width: 950px)
            flex-direction: column-reverse
    
            .flex-wrapper:not(:first-child)
                margin-bottom: 3vh
    
        .inline-flex
            flex-grow: 1
            display: flex
            flex-direction: row
            align-items: center
    
        @media only screen and (max-width: 950px)
            .logo-wrapper
                display: none
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

            @media only screen and (max-width: 950px)
                &
                flex-direction: none
                align-items: end
            .name-signature
                width: 20vh
    
    #signature
        .path-1
            stroke-dasharray: 550
            stroke-dashoffset: 550
        
        .path-2
            stroke-dasharray: 314
            stroke-dashoffset: 314
    
        .path-3
            stroke-dasharray: 45
            stroke-dashoffset: 45
    
        .path-4
            stroke-dasharray: 180
            stroke-dashoffset: 180
    
    </style>