<template>
    <div @click="copy" :style="{ backgroundColor: hex }" class="color">

        <p ref="copy" class="copy">{{copyText}}</p>
        <span ref="label" class="label">{{hex}}</span>

    </div>
</template>

<script>
    export default {
        name: 'Color',
        props: ['hex'],
        data() {
            return {
                copyText: 'COPY'
            }
        },
        methods: {
            copy() {

                var hexInput = document.createElement('input');
                hexInput.setAttribute('value', this.hex);
                hexInput.setAttribute('id', 'hexInput');

                hexInput.style.position = 'fixed';
                hexInput.style.opacity = '0';

                document.body.insertAdjacentElement('beforebegin', hexInput);
                hexInput.select();
                hexInput.setSelectionRange(0, 99999);
                document.execCommand("copy");
                document.body.parentNode.removeChild(document.getElementById('hexInput'));


                this.$refs.copy.style.opacity = 0;

                this.copyText = 'COPIED!'
                
                setTimeout(() => {
                    this.$refs.copy.style.removeProperty('opacity');
                    this.$refs.copy.style.removeProperty('color');
                    this.copyText = 'COPY';
                }, 600);

            }
        }
    }
</script>

<style>
    .color {
        position: relative;
        display: flex;
        flex-direction: column;
        flex: 1 1 20%;
        justify-content: center;
        align-items: center;
        height: 20%;

        outline: 1px solid #e6e6e6;
        overflow: hidden;
        cursor: pointer;
    }

    .color:hover .copy {
        opacity: 1;
    }

    .label {
        position: absolute;
        left: 15px;
        bottom: 15px;
        font-size: 18px;
        color: #a3a1ad;
        letter-spacing: 2.5px;
    }

    .copy {
        transition: opacity .5s;
        opacity: 0;
        color: #f4424a;
        padding: 0px;
        margin: 0px;
        letter-spacing: 1.5px;
        font-size: 2.5vw;
        padding: 2px 8px;
        border-radius: 4px;
    }

    @media screen and (max-width: 600px) {
        .label {
            left: 5px;
            bottom: 5px;
            font-size: 14px;
            letter-spacing: 0px;
        }
    }
</style>