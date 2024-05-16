<template>
    <div
        v-if="products.length > 0"
        v-for="product in products"
        class="sm:p-2 rounded-2xl sm:shadow-card transition-all duration-300 sm:hover:shadow-hover group product_bxx"
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
                class="h-7 w-7 leading-7 radius-10 text-center text-base shadow-badge absolute top-3 right-3 z-10 bg-white"
            >
                <svg
                    width="512"
                    height="512"
                    viewBox="0 0 512 512"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <g clip-path="url(#clip0_5_4)">
                        <path
                            v-if="!product.wishlist"
                            d="M373.437 31C349.414 31.3737 325.916 38.0795 305.314 50.4405C284.713 62.8014 267.738 80.3796 256.103 101.4C244.469 80.3796 227.494 62.8014 206.892 50.4405C186.291 38.0795 162.792 31.3737 138.77 31C100.475 32.6638 64.3957 49.4161 38.4133 77.597C12.4309 105.778 -1.34207 143.097 0.103277 181.4C0.103277 278.403 102.205 384.344 187.837 456.173C206.956 472.24 231.13 481.049 256.103 481.049C281.077 481.049 305.251 472.24 324.37 456.173C410.002 384.344 512.103 278.403 512.103 181.4C513.549 143.097 499.776 105.778 473.793 77.597C447.811 49.4161 411.731 32.6638 373.437 31ZM296.957 423.533C285.522 433.163 271.053 438.443 256.103 438.443C241.154 438.443 226.685 433.163 215.25 423.533C105.639 331.565 42.7699 243.331 42.7699 181.4C41.3116 154.407 50.5858 127.933 68.5697 107.751C86.5536 87.569 111.788 75.3168 138.77 73.6667C165.752 75.3168 190.986 87.569 208.97 107.751C226.954 127.933 236.228 154.407 234.77 181.4C234.77 187.058 237.018 192.484 241.018 196.485C245.019 200.486 250.445 202.733 256.103 202.733C261.761 202.733 267.187 200.486 271.188 196.485C275.189 192.484 277.437 187.058 277.437 181.4C275.978 154.407 285.252 127.933 303.236 107.751C321.22 87.569 346.455 75.3168 373.437 73.6667C400.418 75.3168 425.653 87.569 443.637 107.751C461.621 127.933 470.895 154.407 469.437 181.4C469.437 243.331 406.567 331.565 296.957 423.448V423.533Z"
                            fill="#292929"
                        />

                        <path
                            v-else
                            d="M373.439 22C349.417 22.3665 325.918 28.9434 305.316 41.0667C284.715 53.19 267.739 70.4303 256.105 91.0466C244.471 70.4303 227.495 53.19 206.894 41.0667C186.292 28.9434 162.793 22.3665 138.771 22C100.476 23.6318 64.3961 40.0621 38.4136 67.7012C12.431 95.3403 -1.34208 131.942 0.103278 169.509C0.103278 311.263 233.833 474.988 243.774 481.934L256.105 490.492L268.436 481.934C278.377 475.03 512.107 311.263 512.107 169.509C513.552 131.942 499.779 95.3403 473.796 67.7012C447.814 40.0621 411.734 23.6318 373.439 22Z"
                            fill="#292929"
                        />
                    </g>
                    <defs>
                        <clipPath id="clip0_5_4">
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

        <div class="px-3 sm:px-0 pt-4 pb-2">
            <h3
                class="capitalize text-bs font-semibold whitespace-nowrap transition-all duration-300 hover:text-primary"
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
            <h4 class="sm:text-[22px] font-bold mt-3" v-else>
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
