<template>
    <div @click="open = !open;" class="card" :class="{ active: open }">
        <div class="user-overview">
            <img class="user-overview__image" :src="user.picture.large" alt="">
            <svg v-on:click.stop="callModal" width="48" height="48" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
                <circle cx="24" cy="24" r="24" fill="#009688"/>
                <path opacity="0.54" fill-rule="evenodd" clip-rule="evenodd" d="M15 15C14.447 15 14 15.447 14 16C14 25.388 21.612 33 31 33C31.553 33 32 32.553 32 32V28.5C32 27.948 31.553 27.501 31 27.501C29.752 27.501 28.552 27.3 27.428 26.932C27.328 26.9 27.223 26.884 27.119 26.884C26.864 26.884 26.608 26.982 26.413 27.177L24.212 29.38C21.38 27.94 19.065 25.625 17.623 22.794L19.823 20.587C20.098 20.313 20.179 19.918 20.068 19.572C19.7 18.447 19.499 17.247 19.499 16C19.499 15.447 19.052 15 18.5 15H15Z" fill="white"/>
            </svg>

            <div class="user-overview__inner">
                <p class="user-overview__name">{{ user.name.title + ' ' + user.name.first + ' ' + user.name.last}}</p>
                <p class="user-overview__location">{{ user.location.state + ' ' + user.location.country}}</p>
            </div>
        </div>
        <div class="user-details">
            <div class="user-details__section">
                <img src="@/assets/icons/ghost.png" alt="">
                <div>
                    <p>{{ user.email }}</p>
                    <p>{{ user.cell }}</p>
                    <p>{{ user.phone }}</p>
                </div>
            </div>
            <div class="user-details__section">
                <img src="@/assets/icons/building.png" alt="">
                <div>
                    <p>{{ user.location.street.number + " " + user.location.street.number }}</p>
                    <p>{{ user.location.city }}</p>
                    <p>{{ user.location.state }}</p>
                    <p>{{ user.location.country }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
/*jshint esversion: 6 */
export default {
    name: 'Card',

    props: {
        user: Object
    },

    data: function() {
        return {
            plus: true,
            open: false,
        }
    },

    methods: {
        callModal: function() {
            const vm = this;
            this.$emit('clicked', vm.user);
        }
    }
}
</script>

<style lang="scss">

    .card {
        cursor: pointer;
        margin: 0 auto;
        margin-bottom: .5rem;
        max-width: 344px;
        position: relative;
        text-align: left;
        position: relative;
        transition: all 1s;

        background: #FAFAFA;
        box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.12), 0px 2px 2px rgba(0, 0, 0, 0.24);
        border-radius: 3px;
        color: #616161;



        &.active {
            .user-overview {
                height: 12rem;
                transition: height 1s ease;

                &__image {
                    filter: brightness(0.6);
                    width: 100%;
                    transition: all 1s ease;
                }
                &__inner {
                    padding: 7rem 1rem 2rem 30%;
                    color: white;
                    transition: all 1s ease;
                }

            }


            .user-details {
                max-height: 500px;
                transition: max-height 1s ease;
            }
        }

        .user-overview {
            display: flex;
            overflow: hidden;
            align-items: center;
            height: 5.5rem;
            transition: height 1s ease;
            position: relative;

            &__image {
                position: absolute;
                width: 26%;
                transform-origin: top left;
                transition: all 1s ease;
            }

            svg {
                position: absolute;
                top: 114px;
                left: 27px;
                transition: opacity .5s ease;
                z-index: 10;
                transition: transform .5s ease;

                path {
                    transition: opacity .5s ease;
                }

                &:hover {
                    transform: scale(1.05);
                    transform-origin: center;
                    transition: transform .5s ease;
                    
                    path {
                        opacity: 1;
                        transition: opacity .5s ease;
                    }
                }

            }
            &__inner {
                flex-basis: 100%;
                display: flex;
                flex-direction: column;
                padding: 2rem 1rem 2rem 30%;
                transition: all 1s ease;
                z-index: 5;
            }
            &__name {
                font-size: 1.31rem;
                font-weight: bold;
                margin: 0;
            }

            &__location {
                margin: 0;
                font-weight: normal;
                opacity: 0.8;
                font-size: 14px;
            }
        }

        .user-details {
            max-height: 0;
            overflow-y: hidden;
            transition: max-height 1s ease;
            font-size: .9rem;
            position: relative;

            &__section {
                display: flex;
                margin: 1.25rem;
                justify-content: space-between;
                align-items: flex-start;

                div {
                    padding-left: 1rem;
                    position: relative;
                    flex-basis: 75%;

                    p {
                        margin: 0;
                        line-height: 21px;
                        font-size: 14px;
                    }

                    &::before {
                        content: '';
                        position: absolute;
                        width: 1px;
                        height: 100%;
                        background-color: gray;
                        left: 0;
                        top: 0;
                    }
                }

                img {
                    padding: 0 1.2rem;
                }
            }
        }
    }
</style>

