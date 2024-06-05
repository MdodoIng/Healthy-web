<template>
    <div
        v-if="products.length > 0"
        v-for="product in products"
        class="sm:p-2 rounded-2xl sm:shadow-card transition-all duration-300 sm:hover:shadow-hover group product_bxx grid"
    >
        <div class="relative overflow-hidden rounded-xl isolate">
            <label
                class="capitalize text-xs font-semibold rounded-xl py-1 px-2 shadow-badge absolute top-3 left-3 z-10 bg-secondary text-white"
                v-if="product.is_offer && product.flash_sale"
                >{{ $t("label.flash_sale") }}</label
            >

            <button
                type="button"
                @click.prevent="
                    wishlist(product, (product.wishlist = !product.wishlist))
                "
                :class="product.wishlist ? 'text-primary' : ''"
                class="h-7 w-7 leading-7 radius-10 text-center text-base shadow-badge absolute top-3 right-3 z-10 bg-white flex items-center justify-center p-2"
            >
                <svg
                    v-if="!product.wishlist"
                    xmlns="http://www.w3.org/2000/svg"
                    id="Outline"
                    viewBox="0 0 24 24"
                    width="512"
                    height="512"
                >
                    <path
                        d="M23.836,8.794a3.179,3.179,0,0,0-3.067-2.226H16.4L15.073,2.432a3.227,3.227,0,0,0-6.146,0L7.6,6.568H3.231a3.227,3.227,0,0,0-1.9,5.832L4.887,15,3.535,19.187A3.178,3.178,0,0,0,4.719,22.8a3.177,3.177,0,0,0,3.8-.019L12,20.219l3.482,2.559a3.227,3.227,0,0,0,4.983-3.591L19.113,15l3.56-2.6A3.177,3.177,0,0,0,23.836,8.794Zm-2.343,1.991-4.144,3.029a1,1,0,0,0-.362,1.116L18.562,19.8a1.227,1.227,0,0,1-1.895,1.365l-4.075-3a1,1,0,0,0-1.184,0l-4.075,3a1.227,1.227,0,0,1-1.9-1.365L7.013,14.93a1,1,0,0,0-.362-1.116L2.507,10.785a1.227,1.227,0,0,1,.724-2.217h5.1a1,1,0,0,0,.952-.694l1.55-4.831a1.227,1.227,0,0,1,2.336,0l1.55,4.831a1,1,0,0,0,.952.694h5.1a1.227,1.227,0,0,1,.724,2.217Z"
                    />
                </svg>
                <svg
                    v-else
                    width="512"
                    height="512"
                    viewBox="0 0 512 512"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <g clip-path="url(#clip0_37_14)">
                        <path
                            d="M508.502 187.605C504.138 173.715 495.42 161.598 483.637 153.046C471.853 144.494 457.631 139.961 443.072 140.117H349.867L321.558 51.8823C317.106 37.9935 308.358 25.8773 296.576 17.2811C284.793 8.68483 270.585 4.05273 256 4.05273C241.415 4.05273 227.207 8.68483 215.425 17.2811C203.642 25.8773 194.895 37.9935 190.443 51.8823L162.134 140.117H68.9282C54.4161 140.138 40.2816 144.744 28.5438 153.278C16.8059 161.811 8.06508 173.836 3.56973 187.635C-0.925614 201.433 -0.945511 216.299 3.51288 230.109C7.97128 243.92 16.6799 255.968 28.3949 264.533L104.256 320L75.4135 409.322C70.7525 423.176 70.6934 438.165 75.2452 452.055C79.797 465.945 88.7159 477.992 100.672 486.4C112.424 495.078 126.665 499.727 141.273 499.654C155.881 499.58 170.075 494.789 181.739 485.994L256 431.338L330.283 485.93C342.013 494.559 356.177 499.245 370.739 499.316C385.3 499.387 399.509 494.839 411.323 486.325C423.137 477.812 431.947 465.771 436.486 451.935C441.026 438.099 441.061 423.18 436.587 409.322L407.744 320L483.691 264.533C495.54 256.075 504.351 244.027 508.819 230.172C513.287 216.317 513.176 201.392 508.502 187.605Z"
                            fill="black"
                        />
                    </g>
                    <defs>
                        <clipPath id="clip0_37_14">
                            <rect width="512" height="512" fill="white" />
                        </clipPath>
                    </defs>
                </svg>
            </button>

            <router-link
                class="overflow-hidden rounded-xl w-full"
                :to="{
                    name: 'frontend.product.details',
                    params: { slug: product.slug },
                }"
            >
                <img
                    :src="product.cover"
                    alt="product"
                    class="w-full rounded-xl"
                    loading="lazy"
                />
            </router-link>
        </div>

        <div class="px-2 sm:px-4 py-4 bg-[#BFFFE5] rounded-[10px] grid">
            <h3
                class="capitalize text-[17px] font-semibold line-clamp-2 transition-all duration-300 hover:text-primary"
            >
                <router-link
                    class="block overflow-hidden text-ellipsis"
                    :to="{
                        name: 'frontend.product.details',
                        params: { slug: product.slug },
                    }"
                >
                    {{ product.name }}
                </router-link>
            </h3>

            <!--<div class="flex flex-wrap items-center gap-2 mb-5">
                <div class="flex items-center gap-1">
                    <starRating
                        border-color="#FFBC1F"
                        :rounded-corners="true"
                        :padding="2.5"
                        :border-width="2.5"
                        :star-size="9" class="mt-[2px]" inactive-color="#FFFFFF"
                        active-color="#FFBC1F"
                        :round-start-rating="false" :show-rating="false" :read-only="true"
                        :max-rating="5"
                        :rating="(product.rating_star / product.rating_star_count)"/>
                </div>
                <div v-if="product.rating_star_count > 0" class="flex items-center gap-1 mt-[5px]">
                    <span class="text-xs font-medium whitespace-nowrap text-text">{{ (product.rating_star / product.rating_star_count).toFixed(1) }}</span>
                    <span class="text-xs font-medium whitespace-nowrap text-text hover:text-primary">({{ product.rating_star_count }} {{ product.rating_star_count > 1 ? $t('label.reviews') : $t('label.review') }})</span>
                </div>
            </div>-->

            <div
                class="flex flex-wrap-reverse items-center gap-x-3 gap-y-1"
                v-if="product.is_offer"
            >
                <h3 class="text-2xl sm:text-[22px] font-bold">
                    <span>{{ product.discounted_price }}</span>
                </h3>
                <h4
                    class="text-sm sm:text-base font-semibold text-shopperz-red"
                >
                    <del>{{ product.currency_price }}</del>
                </h4>
            </div>
            <h4 class="sm:text-[24px] font-semibold mt-3" v-else>
                <span>{{ product.currency_price }}</span>
            </h4>
        </div>
    </div>
</template>

<script>
import starRating from "vue-star-rating";
import router from "../../../router";
export default {
    name: "ProductListComponent",
    components: {
        starRating,
    },
    props: {
        products: "object",
    },
    data() {
        return {
            rating: [],
        };
    },
    methods: {
        wishlist: function (product, toggle) {
            this.$store
                .dispatch("frontendWishlist/toggle", {
                    product_id: product.id,
                    toggle: toggle,
                })
                .then((res) => {})
                .catch((err) => {
                    if (err.response.status === 401) {
                        product.wishlist = false;
                        router.push({ name: "auth.login" });
                    }
                });
        },
    },
};
</script>
