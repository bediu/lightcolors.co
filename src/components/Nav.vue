<template>
    <div class="nav">
        <div class="nav-left">
            <svg class="nav-logo" style="fill: #f4424a;" xmlns="http://www.w3.org/2000/svg" height="24"
                viewBox="0 0 24 24" width="24">
                <path
                    d="M12 3c-4.97 0-9 4.03-9 9s4.03 9 9 9c.83 0 1.5-.67 1.5-1.5 0-.39-.15-.74-.39-1.01-.23-.26-.38-.61-.38-.99 0-.83.67-1.5 1.5-1.5H16c2.76 0 5-2.24 5-5 0-4.42-4.03-8-9-8zm-5.5 9c-.83 0-1.5-.67-1.5-1.5S5.67 9 6.5 9 8 9.67 8 10.5 7.33 12 6.5 12zm3-4C8.67 8 8 7.33 8 6.5S8.67 5 9.5 5s1.5.67 1.5 1.5S10.33 8 9.5 8zm5 0c-.83 0-1.5-.67-1.5-1.5S13.67 5 14.5 5s1.5.67 1.5 1.5S15.33 8 14.5 8zm3 4c-.83 0-1.5-.67-1.5-1.5S16.67 9 17.5 9s1.5.67 1.5 1.5-.67 1.5-1.5 1.5z" />
                <path d="M0 0h24v24H0z" fill="none" /></svg>
            <span class="nav-title">lightcolors</span>
        </div>

        <div class="nav-right">
            <div ref="hue" class="nav-hue">
                <div class="hue-drag" @mousedown.prevent="hueDown" @mouseup.prevent="hueUp" @mousemove="hueMove"
                    @mouseleave="hueUp"></div>
                <div :style="{
                left: pickerPosition + 'px'
            }" class="hue-pointer"></div>
            </div>
            <div class="btn-ico" @click="github">
                <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"
                    id="mdi-github-circle" width="24" height="24" viewBox="0 0 24 24">
                    <path
                        d="M12,2A10,10 0 0,0 2,12C2,16.42 4.87,20.17 8.84,21.5C9.34,21.58 9.5,21.27 9.5,21C9.5,20.77 9.5,20.14 9.5,19.31C6.73,19.91 6.14,17.97 6.14,17.97C5.68,16.81 5.03,16.5 5.03,16.5C4.12,15.88 5.1,15.9 5.1,15.9C6.1,15.97 6.63,16.93 6.63,16.93C7.5,18.45 8.97,18 9.54,17.76C9.63,17.11 9.89,16.67 10.17,16.42C7.95,16.17 5.62,15.31 5.62,11.5C5.62,10.39 6,9.5 6.65,8.79C6.55,8.54 6.2,7.5 6.75,6.15C6.75,6.15 7.59,5.88 9.5,7.17C10.29,6.95 11.15,6.84 12,6.84C12.85,6.84 13.71,6.95 14.5,7.17C16.41,5.88 17.25,6.15 17.25,6.15C17.8,7.5 17.45,8.54 17.35,8.79C18,9.5 18.38,10.39 18.38,11.5C18.38,15.32 16.04,16.16 13.81,16.41C14.17,16.72 14.5,17.33 14.5,18.26C14.5,19.6 14.5,20.68 14.5,21C14.5,21.27 14.66,21.59 15.17,21.5C19.14,20.16 22,16.42 22,12A10,10 0 0,0 12,2Z" />
                </svg>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        name: 'Nav',
        props: ['hue'],
        data() {
            return {
                isDragging: false,
                pickerPosition: 0
            }
        },
        computed: {
            hueValue: {
                get() {
                    return this.hue
                },
                set(n) {
                    this.$emit('newHue', n);
                }
            }
        },
        methods: {
            hueDown(e) {

                this.isDragging = true;
                this.pickerPosition = e.offsetX - 8;
                this.hueValue = e.offsetX * (360 / this.$refs.hue.scrollWidth);

            },
            hueUp() {
                this.isDragging = false;
            },
            hueMove(e) {
                if (e && this.isDragging) {
                    
                    if (e.offsetX > 0) {
                        this.pickerPosition = e.offsetX - 8;
                    this.hueValue = e.offsetX * (360 / this.$refs.hue.scrollWidth);
                    }
                    
                } else {
                    this.isDragging = false;
                }
            },
            _offset(e) {
                var rect = e.target.getBoundingClientRect();
                var bodyRect = document.body.getBoundingClientRect();
                var x = e.touches[0].pageX - (rect.left - bodyRect.left);
                var y = e.touches[0].pageY - (rect.top - bodyRect.top);
                return [x, y];
            },
            github() {
                window.open('https://github.com/bediu/lightcolors.co','_blank');
            }
        },
        mounted() {
            this.pickerPosition = this.hue / (360 / this.$refs.hue.scrollWidth) - 8;
        }
    }
</script>

<style>
    /* nav */
    .nav {
        position: fixed;
        display: flex;
        justify-content: space-between;
        align-items: center;
        top: 0px;
        left: 0px;
        width: 100%;
        height: 55px;
        box-shadow: 0px 2px 4px -1px rgba(0, 0, 0, 0.2), 0px 4px 5px 0px rgba(0, 0, 0, 0.14), 0px 1px 10px 0px rgba(0, 0, 0, 0.12);
        z-index: 10;
        background: #fcfcfc;
    }

    .nav-left {
        display: inherit;
        align-items: center;
        padding-left: 25px;
    }

    .nav-right {
        display: inherit;
        align-items: center;
        padding-right: 25px;
    }

    .nav-logo {
        fill: #f4424a;
        margin-right: 7px;
    }

    .nav-title {
        padding: 0px 25px;
        color: #0a0f3c;
        font-weight: 600;
        font-size: 20px;
        line-height: 1.5;
    }


    /* hue */
    .nav-hue {
        position: relative;
        display: flex;

        height: 10px;
        width: 250px;
        max-width: 250px;

        align-items: center;
        border-radius: 4px;
        background: linear-gradient(90deg, red 0, #ff0 17%, #0f0 33%, #0ff 50%, #00f 67%, #f0f 83%, red);
        cursor: pointer;
        opacity: 0.9;
    }

    .hue-drag {
        position: absolute;
        width: 100%;
        height: 32px;
        top: 0;
        left: 0;
        cursor: pointer;
        background: transparent;
        z-index: 1;
    }

    .hue-pointer {
        position: absolute;
        left: 0px;
        width: 16px;
        height: 16px;
        background: rgba(255, 255, 255, 0.9);
        border-radius: 5px;
        box-shadow: 0px 0px 2px 0px rgba(0, 0, 0, 0.9);
    }



    /* nav github */
    .btn-ico {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 45px;
        height: 45px;
        margin-left: 25px;
        border-radius: 50%;
        transition: all .3s;
        cursor: pointer;
    }

    .btn-ico>svg {
        padding: 0px;
        fill: #0a0f3c;
    }

    .btn-ico:hover {
        background: rgba(0, 0, 0, 0.1);
    }

    .btn-ico:active {
        background: rgba(0, 0, 0, 0.3);
    }


    @media screen and (max-width: 600px) {
 
        .btn-ico {
            display: none;
        }
        .nav-right {
            flex: 1;
        }

        .btn-ico {
            margin-left: 45px;
        }

        .nav-hue {
            width: 100%;
        }
    }
</style>