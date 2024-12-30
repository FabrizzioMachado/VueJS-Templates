<template>
    <nav class="navbar" :style="{ color: navbarInfo.navbarColor, backgroundColor: navbarBackground }">
        <div class="container-nav">
            <div class="logo">
                <a :href="navbarInfo.titleLogo[1]" id="logoLink">
                    {{ navbarInfo.titleLogo[0] }}
                </a>
            </div>
            <ul class="nav-links">
                <li v-for="(slot, index) in navbarInfo.slots" :key="index" :class="{ 'active': currentSection === index }">
                    <a :href="slot.link">{{ slot.text }}</a>
                </li>
            </ul>
            <div class="burguer">
                <i id="burguerIcon" 
                    class="bi bi-list"
                    @click="burguerClick()" 
                ></i>
            </div>
        </div>
    </nav>
</template>

<script>
    export default {
        name: 'Navbar',
        props: {
            navbarInfo: {
                type: Object,
                required: true
            },
        },
        data() {
            return {
                currentSection: 0,
                navbarBackground: 'transparent',
            };
        },
        mounted() {
            window.addEventListener('scroll', this.handleScroll);
            window.addEventListener('scroll', this.updateCurrentSection);
            this.updateCurrentSection();
        },
        beforeDestroy() {
            window.removeEventListener('scroll', this.handleScroll);
            window.removeEventListener('scroll', this.updateCurrentSection);
        },
        methods: {
            handleScroll() {
                this.navbarBackground = window.scrollY > window.innerHeight ? this.navbarInfo.navbarBgColor : 'transparent';
            },
            updateCurrentSection() {
                const sections = this.navbarInfo.slots.map(slot => document.querySelector(slot.link));
                const scrollPosition = window.scrollY + window.innerHeight / 2;

                sections.forEach((section, index) => {
                    if (section) {
                        const rect = section.getBoundingClientRect();
                        const sectionTop = rect.top + window.scrollY;
                        const sectionBottom = sectionTop + section.offsetHeight;

                        if (scrollPosition >= sectionTop && scrollPosition <= sectionBottom) {
                            this.currentSection = index;
                        }
                    }
                });
            },
            burguerClick() {
                const burguerIcon = document.querySelector('#burguerIcon');
                burguerIcon.classList.toggle('bi-x');
                burguerIcon.classList.toggle('bi-list');

                const navLinks = document.querySelector('.nav-links');
                navLinks.style.height = burguerIcon.classList.contains('bi-x') ? '100vh' : '0';
            },
        }
    };
</script>

<style lang="scss">
    @import url('https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap');
    $font-family: "Josefin Sans";

    .navbar {
        width: 100vw;
        height: 3.5em;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        position: fixed;
        top: 0;
        left: 0;
        z-index: 1000;
        font-family: $font-family;

        *{
            transition: all 0.3s ease-in-out;
        }

        .container-nav {
            width: 100vw;
            height: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: relative;

            .logo, .nav-links, .burguer {
                font-weight: bold;
                a, i{
                    text-decoration: none;
                    color: white;

                    &:hover {
                        color: v-bind(color) !important;
                        cursor: pointer;
                    }
                } 
            }

            .logo {
                font-size: 1.5em;
                position: absolute;
                left: calc(1.25em + 0.25vw);
                top: 0.7em;
                z-index: 10;
            }


            .nav-links {
                width: 100vw;
                height: 0;
                overflow: hidden;
                display: flex;
                flex-direction: column;
                background: rgba(0, 0, 0, 0.75);
                backdrop-filter: blur(10em);
                position: absolute;
                top: 0;
                left: 0;
                align-items: center;
                justify-content: center;
                gap: clamp(1.5em, 5vw - 5em, 3em);
                list-style: none;
                padding: 0;
                margin: 0;
                font-size: clamp(1.25em, 7.5vw - 2.5em, 1em);

                li {
                    margin: 0 25px;

                    &.active a {
                        color: v-bind(color);
                    }
                }
            }

            .burguer {
                position: absolute;
                top: 1.25em;
                right: calc(2.5em + 5vw);
                height: 100%;
                transform: translateY(-0.5em);
                z-index: 10;
                
                i {
                    font-size: 2em; 
                    cursor: pointer;
                    position: absolute;
                    top: 0;
                }
            }
        }
    }

    @media only screen and (min-width: 992px) {
        .navbar {
            .container-nav{
                width: min(90%, 90em) !important;

                .logo {
                    left: 0 !important;
                }
            }

            .nav-links {
                flex-direction: row !important;
                justify-content: right !important;
                width: 100% !important;
                height: 100% !important;
                background: none !important;
                gap: 0 !important;
                backdrop-filter: none !important;
            }
            .burguer {
                display: none;
            }
        }
    }
</style>