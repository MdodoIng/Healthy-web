<template>
    <LoadingComponent :props="loading" />
    <section class="mb-12">
        <div class="container">
            <div class="row">
                <div class="col-12">
                    <CategoryBreadcrumbComponent :categories="categories" />
                </div>

                <div v-if="images.length" class="col-12 sm:col-6 lg:col-5">
                    <div class="">
                        <Swiper
                            dir="ltr"
                            :spaceBetween="10"
                            :navigation="true"
                            :thumbs="{ swiper: thumbsSwiper }"
                            :modules="modules"
                            class="gallery-swiper"
                        >
                            <SwiperSlide
                                v-for="(image, index) in images"
                                :key="index"
                                class="w-full"
                            >
                                <img
                                    class="w-full rounded-2xl"
                                    :src="image"
                                    alt="gallery"
                                    loading="lazy"
                                />
                            </SwiperSlide>
                        </Swiper>

                        <Swiper
                            dir="ltr"
                            @swiper="setThumbsSwiper"
                            :spaceBetween="12"
                            :slidesPerView="4"
                            :freeMode="true"
                            :watchSlidesProgress="true"
                            :modules="modules"
                            class="thumb-swiper"
                        >
                            <SwiperSlide
                                v-for="(image, index) in images"
                                :key="index"
                                class="w-full cursor-pointer rounded-lg border border-gray-200 transition-all duration-500"
                            >
                                <img
                                    class="w-full rounded-lg border-2 border-gray-200 transition-all duration-500"
                                    :src="image"
                                    alt="gallery"
                                    loading="lazy"
                                />
                            </SwiperSlide>
                        </Swiper>
                    </div>
                </div>

                <div v-else class="col-12 sm:col-6 lg:col-5">
                    <div class="product_bx">
                        <img
                            :src="product.image"
                            alt="products"
                            class="w-full rounded-2xl"
                            loading="lazy"
                        />
                    </div>
                </div>

                <div class="col-12 sm:col-6 lg:col-7 lg:pl-10 product-clm">
                    <div class="detail_bx">
                        <h2
                            class="text-3xl sm:text-4xl font-bold capitalize mb-5"
                        >
                            {{ product.name }}
                        </h2>
                        <h3 class="flex items-start gap-4 mb-5">
                            <span class="text-2xxl font-bold">
                                {{
                                    currencyFormat(
                                        temp.price,
                                        setting.site_digit_after_decimal_point,
                                        setting.site_default_currency_symbol,
                                        setting.site_currency_position
                                    )
                                }}
                            </span>
                            <del
                                v-if="product.is_offer"
                                class="text-lg font-bold text-shopperz-red"
                            >
                                {{
                                    currencyFormat(
                                        temp.oldPrice,
                                        setting.site_digit_after_decimal_point,
                                        setting.site_default_currency_symbol,
                                        setting.site_currency_position
                                    )
                                }}
                            </del>
                        </h3>

                        <!-- <div class="flex flex-wrap items-center gap-2 border-b border-gray-100 mb-6 pb-6">
                        <starRating border-color="#FFBC1F" :rounded-corners="true" :padding="2.5" :border-width="2.5"
                                    :star-size="11" class="-mt-0.5" inactive-color="#FFFFFF" active-color="#FFBC1F"
                                    :round-start-rating="false" :show-rating="false" :read-only="true" :max-rating="5"
                                    :rating="(product.rating_star / product.rating_star_count)"/>
                        <div v-if="product.rating_star_count > 0" class="flex items-center gap-1">
                                <span class="text-base font-medium whitespace-nowrap text-text">
                                    {{ (product.rating_star / product.rating_star_count).toFixed(1) }}
                                </span>
                            <span
                                class="text-base font-medium whitespace-nowrap text-text hover:text-primary cursor-pointer">
                                ({{
                                    product.rating_star_count
                                }} {{ product.rating_star_count > 1 ? $t('label.reviews') : $t('label.review') }})
                            </span>
                        </div>
                    </div>-->

                        <VariationComponent
                            v-if="
                                initialVariations.length > 0 &&
                                variationComponent
                            "
                            :method="selectedVariationMethod"
                            :variations="initialVariations"
                        />

                        <dl
                            class="flex flex-wrap items-center gap-x-6 gap-y-3 mb-8"
                        >
                            <dt class="capitalize text-lg font-semibold">
                                {{ $t("label.quantity") }}:
                            </dt>
                            <dd class="flex items-center gap-6">
                                <div
                                    class="flex items-center gap-1 w-25 p-2 radius-10 bg-[#F7F7FC]"
                                >
                                    <button
                                        @click.prevent="quantityDecrement"
                                        type="button"
                                        :class="
                                            temp.quantity === 1
                                                ? 'cursor-not-allowed'
                                                : ''
                                        "
                                        class="lab-fill-circle-minus text-lg leading-none transition-all duration-300 hover:text-primary"
                                    ></button>
                                    <input
                                        type="number"
                                        v-model="temp.quantity"
                                        v-on:keypress="onlyNumber($event)"
                                        v-on:keyup="quantityUp"
                                        class="text-center w-full h-5 text-sm font-medium"
                                    />
                                    <button
                                        @click.prevent="quantityIncrement"
                                        type="button"
                                        :class="
                                            temp.stock === temp.quantity
                                                ? 'cursor-not-allowed'
                                                : ''
                                        "
                                        class="lab-fill-circle-plus text-lg leading-none transition-all duration-300 hover:text-primary"
                                    ></button>
                                </div>
                                <div
                                    v-if="
                                        !initialVariations.length ||
                                        selectedVariation != null
                                    "
                                >
                                    <p v-if="temp.stock > 0" class="capitalize">
                                        {{ $t("label.available") }}:
                                        <b>({{ temp.stock }}) </b>
                                        {{ product.unit }}
                                    </p>
                                    <p v-else class="capitalize text-danger">
                                        {{ $t("label.stock_out") }}
                                    </p>
                                </div>
                            </dd>
                        </dl>

                        <dl
                            v-if="temp.quantity > 1"
                            class="flex flex-wrap items-center gap-x-6 gap-y-3 mb-10"
                        >
                            <dt class="capitalize text-lg font-semibold">
                                {{ $t("label.total_price") }}:
                            </dt>
                            <dd
                                class="flex items-center gap-6 text-green-500 font-semibold text-lg"
                            >
                                {{
                                    currencyFormat(
                                        temp.totalPrice,
                                        setting.site_digit_after_decimal_point,
                                        setting.site_default_currency_symbol,
                                        setting.site_currency_position
                                    )
                                }}
                            </dd>
                        </dl>

                        <div
                            class="flex flex-wrap items-center gap-8 mb-10 pt-5"
                        >
                            <button
                                @click.prevent="addToCart"
                                :disabled="enableAddToCardButton"
                                type="button"
                                :class="
                                    enableAddToCardButton === false
                                        ? 'bg-primary'
                                        : ''
                                "
                                class="flex items-center gap-3 px-8 h-12 leading-12 radius-10 transition-all duration-500 bg-slate-400 text-black hover:bg-primary"
                            >
                                <img
                                    class="icon_titl"
                                    src="public/images/default/icon/cart-shopping-fast.svg"
                                />
                                <span class="whitespace-nowrap font-bold">{{
                                    $t("button.add_to_cart")
                                }}</span>
                            </button>
                            <button
                                type="button"
                                @click="
                                    wishlist(
                                        (product.wishlist = !product.wishlist)
                                    )
                                "
                                :class="
                                    product.wishlist
                                        ? 'text-primary'
                                        : 'text-secondary'
                                "
                                class="flex items-center gap-3 px-8 h-12 leading-12 radius-10 transition-all duration-500 shadow-btn-secondary bg-white fvt-btn"
                            >
                                <i
                                    :class="
                                        product.wishlist
                                            ? 'lab-fill-heart'
                                            : 'lab-line-heart'
                                    "
                                    class="lab-line-heart text-xl"
                                ></i>

                                <svg
                                    width="512"
                                    height="512"
                                    viewBox="0 0 512 512"
                                    fill="none"
                                    xmlns="http://www.w3.org/2000/svg"
                                >
                                    <g clip-path="url(#clip0_5_4)">
                                        <path
                                            v-if="product.wishlist"
                                            d="M373.439 22C349.417 22.3665 325.918 28.9434 305.316 41.0667C284.715 53.19 267.739 70.4303 256.105 91.0466C244.471 70.4303 227.495 53.19 206.894 41.0667C186.292 28.9434 162.793 22.3665 138.771 22C100.476 23.6318 64.3961 40.0621 38.4136 67.7012C12.431 95.3403 -1.34208 131.942 0.103278 169.509C0.103278 311.263 233.833 474.988 243.774 481.934L256.105 490.492L268.436 481.934C278.377 475.03 512.107 311.263 512.107 169.509C513.552 131.942 499.779 95.3403 473.796 67.7012C447.814 40.0621 411.734 23.6318 373.439 22Z"
                                            fill="#292929"
                                        />

                                        <path
                                            v-else
                                            d="M373.437 31C349.414 31.3737 325.916 38.0795 305.314 50.4405C284.713 62.8014 267.738 80.3796 256.103 101.4C244.469 80.3796 227.494 62.8014 206.892 50.4405C186.291 38.0795 162.792 31.3737 138.77 31C100.475 32.6638 64.3957 49.4161 38.4133 77.597C12.4309 105.778 -1.34207 143.097 0.103277 181.4C0.103277 278.403 102.205 384.344 187.837 456.173C206.956 472.24 231.13 481.049 256.103 481.049C281.077 481.049 305.251 472.24 324.37 456.173C410.002 384.344 512.103 278.403 512.103 181.4C513.549 143.097 499.776 105.778 473.793 77.597C447.811 49.4161 411.731 32.6638 373.437 31ZM296.957 423.533C285.522 433.163 271.053 438.443 256.103 438.443C241.154 438.443 226.685 433.163 215.25 423.533C105.639 331.565 42.7699 243.331 42.7699 181.4C41.3116 154.407 50.5858 127.933 68.5697 107.751C86.5536 87.569 111.788 75.3168 138.77 73.6667C165.752 75.3168 190.986 87.569 208.97 107.751C226.954 127.933 236.228 154.407 234.77 181.4C234.77 187.058 237.018 192.484 241.018 196.485C245.019 200.486 250.445 202.733 256.103 202.733C261.761 202.733 267.187 200.486 271.188 196.485C275.189 192.484 277.437 187.058 277.437 181.4C275.978 154.407 285.252 127.933 303.236 107.751C321.22 87.569 346.455 75.3168 373.437 73.6667C400.418 75.3168 425.653 87.569 443.637 107.751C461.621 127.933 470.895 154.407 469.437 181.4C469.437 243.331 406.567 331.565 296.957 423.448V423.533Z"
                                            fill="#292929"
                                        />
                                    </g>
                                    <defs>
                                        <clipPath id="clip0_5_4">
                                            <rect
                                                width="512"
                                                height="512"
                                                fill="white"
                                            />
                                        </clipPath>
                                    </defs>
                                </svg>

                                <span class="whitespace-nowrap font-bold">{{
                                    $t("button.favorite")
                                }}</span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section class="mb-12">
        <div class="container">
            <div class="row">
                <div class="col-12">
                    <div class="rounded-[32px] border border-[#D9DBE9]">
                        <nav
                            class="flex flex-wrap items-center p-4 sm:py-6 sm:px-8 gap-3 sm:gap-6"
                        >
                            <button
                                type="button"
                                @click.prevent="
                                    multiTargets(
                                        $event,
                                        'tab-btn',
                                        'tab-div',
                                        'tab_details'
                                    )
                                "
                                class="tab-btn active text-sm sm:text-base font-bold leading-5 capitalize py-2 sm:py-3.5 px-5 sm:px-8 radius-10 border border-[#D9DBE9]"
                            >
                                {{ $t("label.details") }}
                            </button>

                            <button
                                type="button"
                                @click.prevent="
                                    multiTargets(
                                        $event,
                                        'tab-btn',
                                        'tab-div',
                                        'tab_videos'
                                    )
                                "
                                class="tab-btn text-sm sm:text-base font-bold leading-5 capitalize py-2 sm:py-3.5 px-5 sm:px-8 radius-10 border border-[#D9DBE9]"
                            >
                                {{ $t("label.videos") }}
                            </button>

                            <button
                                type="button"
                                @click.prevent="
                                    multiTargets(
                                        $event,
                                        'tab-btn',
                                        'tab-div',
                                        'tab_reviews'
                                    )
                                "
                                class="tab-btn text-sm sm:text-base font-bold leading-5 capitalize py-2 sm:py-3.5 px-5 sm:px-8 radius-10 border border-[#D9DBE9]"
                            >
                                {{ $t("label.reviews") }}
                            </button>
                            <button
                                type="button"
                                @click.prevent="
                                    multiTargets(
                                        $event,
                                        'tab-btn',
                                        'tab-div',
                                        'tab_shipping_and_return'
                                    )
                                "
                                class="tab-btn text-sm sm:text-base font-bold leading-5 capitalize py-2 sm:py-3.5 px-5 sm:px-8 radius-10 border border-[#D9DBE9]"
                            >
                                {{ $t("label.shipping_and_return") }}
                            </button>
                        </nav>

                        <div
                            id="tab_details"
                            class="tab-div active p-4 sm:p-8 sm:pt-6 border-t border-[#D9DBE9]"
                        >
                            <h3
                                class="capitalize text-2xl sm:text-3xl font-bold mb-4"
                            >
                                {{ $t("label.product_details") }}
                            </h3>
                            <div
                                class="text-description"
                                v-html="product.details"
                            ></div>
                        </div>

                        <div
                            id="tab_videos"
                            class="tab-div p-4 sm:p-8 sm:pt-6 border-t border-[#D9DBE9]"
                        >
                            <h3
                                class="capitalize text-2xl sm:text-3xl font-bold mb-4"
                            >
                                {{ $t("label.product_videos") }}
                            </h3>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                                <iframe
                                    v-for="video in videos"
                                    :src="video.link"
                                    class="w-full h-40 sm:h-64 rounded-2xl"
                                ></iframe>
                            </div>
                        </div>

                        <div
                            id="tab_reviews"
                            class="tab-div p-4 sm:p-8 sm:pt-6 border-t border-[#D9DBE9]"
                        >
                            <h3
                                class="capitalize text-2xl sm:text-3xl font-bold mb-4"
                            >
                                {{ $t("label.product_reviews") }}
                            </h3>
                            <div
                                class="flex flex-wrap items-center gap-2 border-b border-gray-100 mb-6 pb-6"
                            >
                                <starRating
                                    border-color="#FFBC1F"
                                    :rounded-corners="true"
                                    :padding="2.5"
                                    :border-width="2.5"
                                    :star-size="11"
                                    class="-mt-0.5"
                                    inactive-color="#FFFFFF"
                                    active-color="#FFBC1F"
                                    :round-start-rating="false"
                                    :show-rating="false"
                                    :read-only="true"
                                    :max-rating="5"
                                    :rating="
                                        product.rating_star /
                                        product.rating_star_count
                                    "
                                />
                                <div
                                    v-if="product.rating_star_count > 0"
                                    class="flex items-center gap-1"
                                >
                                    <span
                                        class="text-base font-medium whitespace-nowrap text-text"
                                    >
                                        {{
                                            (
                                                product.rating_star /
                                                product.rating_star_count
                                            ).toFixed(1)
                                        }}
                                    </span>
                                    <span
                                        class="text-base font-medium whitespace-nowrap text-text hover:text-primary cursor-pointer"
                                    >
                                        ({{ product.rating_star_count }}
                                        {{
                                            product.rating_star_count > 1
                                                ? $t("label.reviews")
                                                : $t("label.review")
                                        }})
                                    </span>
                                </div>
                            </div>

                            <div v-for="review in reviews" class="mb-8">
                                <h4
                                    class="text-lg font-semibold capitalize mb-2"
                                >
                                    {{ review.name }}
                                </h4>
                                <div
                                    class="flex flex-wrap items-center gap-2 mb-3"
                                >
                                    <starRating
                                        border-color="#FFBC1F"
                                        inactive-color="#FFFFFF"
                                        active-color="#FFBC1F"
                                        :rounded-corners="true"
                                        :padding="2.5"
                                        :border-width="2.5"
                                        :star-size="11"
                                        class="-mt-0.5"
                                        :round-start-rating="false"
                                        :show-rating="false"
                                        :read-only="true"
                                        :max-rating="5"
                                        :rating="review.star"
                                    />
                                </div>
                                <p class="mb-4">{{ review.review }}</p>

                                <div
                                    class="flex flex-wrap gap-4"
                                    v-if="review.images.length > 0"
                                >
                                    <img
                                        v-for="reviewImage in review.images"
                                        :src="reviewImage"
                                        alt="image"
                                        class="w-20 rounded-lg"
                                    />
                                </div>
                            </div>

                            <button
                                v-if="
                                    product.rating_star_count > reviews.length
                                "
                                @click.prevent="readMore"
                                type="button"
                                class="flex items-center justify-center gap-3 w-fit mx-auto text-primary"
                            >
                                <span class="text-lg font-medium capitalize">{{
                                    $t("label.read_more")
                                }}</span>
                                <i
                                    class="lab-line-down-arrow text-sm -mt-1"
                                ></i>
                            </button>
                        </div>

                        <div
                            id="tab_shipping_and_return"
                            class="tab-div p-4 sm:p-8 sm:pt-6 border-t border-[#D9DBE9]"
                        >
                            <h3
                                class="capitalize text-2xl sm:text-3xl font-bold mb-4"
                            >
                                {{ $t("label.product_shipping_and_return") }}
                            </h3>
                            <div
                                class="text-description"
                                v-html="product.shipping_and_return"
                            ></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section v-if="relatedProducts.length > 0" class="mb-10 sm:mb-20">
        <div class="container">
            <div class="flex items-center justify-between gap-4 mb-5 sm:mb-7">
                <h2 class="text-2xl sm:text-4xl font-bold capitalize title">
                    {{ $t("label.related_products") }}
                </h2>
            </div>
            <div
                class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 sm:gap-6 pt-3"
            >
                <ProductListComponent
                    v-if="relatedProducts.length > 0"
                    :products="relatedProducts"
                />
            </div>
        </div>
    </section>
</template>

<script>
import { ref } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import { FreeMode, Navigation, Thumbs } from "swiper/modules";
import LoadingComponent from "../components/LoadingComponent";
import starRating from "vue-star-rating";
import targetService from "../../../services/targetService";
import router from "../../../router";
import CategoryBreadcrumbComponent from "../components/CategoryBreadcrumbComponent";
import ProductListComponent from "../components/ProductListComponent";
import VariationComponent from "../components/VariationComponent";
import appService from "../../../services/appService";
import alertService from "../../../services/alertService";
import { useHead } from "@vueuse/head";

export default {
    name: "ProductDetailsComponent",
    components: {
        VariationComponent,
        ProductListComponent,
        CategoryBreadcrumbComponent,
        starRating,
        Swiper,
        SwiperSlide,
        LoadingComponent,
    },
    setup() {
        const thumbsSwiper = ref(null);
        const setThumbsSwiper = (swiper) => {
            thumbsSwiper.value = swiper;
        };
        return {
            thumbsSwiper,
            setThumbsSwiper,
            modules: [FreeMode, Navigation, Thumbs],
        };
    },
    data() {
        return {
            loading: {
                isActive: false,
            },
            props: {
                search: {
                    slug: null,
                    review_limit: 3,
                },
            },
            enableAddToCardButton: true,
            selectedVariation: null,
            productArray: {},
            variationComponent: false,
            initProduct: {
                isVariation: false,
                variationId: null,
                sku: null,
                stock: 0,
                quantity: 1,
                discount: 0,
                price: 0,
                oldPrice: 0,
                totalPrice: 0,
            },
            temp: {
                name: "",
                image: "",
                isVariation: false,
                variationId: null,
                productId: 0,
                sku: null,
                stock: 0,
                taxes: {},
                shipping: {},
                quantity: 1,
                discount: 0,
                price: 0,
                oldPrice: 0,
                totalPrice: 0,
            },
        };
    },
    computed: {
        setting: function () {
            return this.$store.getters["frontendSetting/lists"];
        },
        categories: function () {
            return this.$store.getters[
                "frontendProductCategory/ancestorsAndSelf"
            ];
        },
        initialVariations: function () {
            return this.$store.getters[
                "frontendProductVariation/initialVariation"
            ];
        },
        product: function () {
            return this.$store.getters["frontendProduct/show"];
        },
        images: function () {
            return this.$store.getters["frontendProduct/showImages"];
        },
        videos: function () {
            return this.$store.getters["frontendProduct/showVideos"];
        },
        reviews: function () {
            return this.$store.getters["frontendProduct/showReviews"];
        },
        relatedProducts: function () {
            return this.$store.getters["frontendProduct/relatedProducts"];
        },
    },
    mounted() {
        this.show();
        this.showRelatedProduct();
    },
    methods: {
        onlyNumber: function (e) {
            return appService.onlyNumber(e);
        },
        currencyFormat: function (amount, decimal, currency, position) {
            return appService.currencyFormat(
                amount,
                decimal,
                currency,
                position
            );
        },
        multiTargets: function (
            event,
            commonBtnClass,
            commonDivClass,
            targetID
        ) {
            targetService.multiTargets(
                event,
                commonBtnClass,
                commonDivClass,
                targetID
            );
        },
        wishlist: function (toggle) {
            this.$store
                .dispatch("frontendWishlist/toggle", {
                    product_id: this.product.id,
                    toggle: toggle,
                })
                .then((res) => {})
                .catch((err) => {
                    if (err.response.status === 401) {
                        this.product.wishlist = false;
                        router.push({ name: "auth.login" });
                    }
                });
        },
        readMore: function () {
            this.props.search.review_limit += 1;
            this.show();
        },
        show: function () {
            if (typeof this.$route.params.slug !== "undefined") {
                this.loading.isActive = true;
                this.props.search.slug = this.$route.params.slug;
                this.$store
                    .dispatch("frontendProduct/show", this.props.search)
                    .then((res) => {
                        this.initProduct = {
                            isVariation: false,
                            variationId: null,
                            sku: res.data.data.sku,
                            stock: res.data.data.stock,
                            quantity: 1,
                            discount: 0,
                            price: res.data.data.price,
                            oldPrice: res.data.data.old_price,
                            totalPrice: res.data.data.price,
                        };
                        this.temp = {
                            name: res.data.data.name,
                            image: res.data.data.image,
                            isVariation: false,
                            variationId: null,
                            productId: res.data.data.id,
                            sku: res.data.data.sku,
                            stock: res.data.data.stock,
                            taxes: res.data.data.taxes,
                            shipping: res.data.data.shipping,
                            quantity: 1,
                            discount: 0,
                            price: res.data.data.price,
                            oldPrice: res.data.data.old_price,
                            totalPrice: res.data.data.price,
                        };

                        this.$store
                            .dispatch(
                                "frontendProductCategory/ancestorsAndSelf",
                                res.data.data.category_slug
                            )
                            .then((categoryRes) => {
                                this.loading.isActive = false;
                            })
                            .catch((err) => {
                                this.loading.isActive = false;
                            });

                        this.$store
                            .dispatch(
                                "frontendProductVariation/initialVariation",
                                res.data.data.id
                            )
                            .then((initVariationRes) => {
                                if (initVariationRes.data.data.length > 0) {
                                    this.variationComponent = true;
                                }

                                if (
                                    !initVariationRes.data.data.length &&
                                    res.data.data.stock > 0
                                ) {
                                    this.enableAddToCardButton = false;
                                }
                                this.loading.isActive = false;
                            })
                            .catch((err) => {
                                this.loading.isActive = false;
                            });

                        if (
                            Object.keys(res.data.data.seo) &&
                            res.data.data.seo.title &&
                            res.data.data.seo.description
                        ) {
                            let metaData = [
                                {
                                    name: "title",
                                    content: res.data.data.seo.title,
                                },
                                {
                                    name: "description",
                                    content: res.data.data.seo.description,
                                },
                            ];

                            if (
                                res.data.data.seo.thumb &&
                                res.data.data.seo.cover
                            ) {
                                metaData.push({
                                    content: res.data.data.seo.thumb,
                                });
                                metaData.push({
                                    content: res.data.data.seo.cover,
                                });
                            }

                            useHead({
                                title:
                                    this.setting.company_name +
                                    " - " +
                                    res.data.data.seo.title,
                                meta: metaData,
                            });
                        }
                    })
                    .catch((err) => {
                        this.loading.isActive = false;
                    });
            }
        },
        showRelatedProduct: function () {
            if (typeof this.$route.params.slug !== "undefined") {
                this.loading.isActive = true;
                this.props.search.slug = this.$route.params.slug;
                this.$store
                    .dispatch("frontendProduct/relatedProducts", {
                        slug: this.$route.params.slug,
                        rand: 8,
                    })
                    .then((res) => {
                        this.loading.isActive = false;
                    })
                    .catch((err) => {
                        this.loading.isActive = false;
                    });
            }
        },
        selectedVariationMethod: function (variation) {
            this.enableAddToCardButton = true;
            this.selectedVariation = null;

            this.temp.isVariation = this.initProduct.isVariation;
            this.temp.variationId = this.initProduct.variationId;
            this.temp.sku = this.initProduct.sku;
            this.temp.stock = this.initProduct.stock;
            this.temp.quantity = this.initProduct.quantity;
            this.temp.discount = this.initProduct.discount;
            this.temp.price = this.initProduct.price;
            this.temp.oldPrice = this.initProduct.oldPrice;
            this.temp.totalPrice = this.initProduct.price;

            if (variation) {
                this.selectedVariation = variation;

                this.temp.isVariation = true;
                this.temp.variationId = variation.id;
                this.temp.sku = variation.sku;
                this.temp.stock = variation.stock;
                this.temp.quantity = 1;
                this.temp.discount = 0;
                this.temp.price = variation.price;
                this.temp.oldPrice = variation.old_price;
                this.temp.totalPrice = variation.price;

                if (variation.stock > 0) {
                    this.enableAddToCardButton = false;
                }
            }
        },
        quantityUp: function () {
            if (this.temp.quantity === 0) {
                this.temp.quantity = 1;
            }
            if (this.temp.quantity > this.temp.stock) {
                this.temp.quantity = this.temp.stock;
            }
            this.totalPriceSetup();
        },
        quantityIncrement: function () {
            this.temp.quantity++;
            if (this.temp.quantity <= 0) {
                this.temp.quantity = 1;
            }

            if (this.temp.quantity > this.temp.stock) {
                this.temp.quantity--;
            }
            this.totalPriceSetup();
        },
        quantityDecrement: function () {
            this.temp.quantity--;
            if (this.temp.quantity <= 0) {
                this.temp.quantity = 1;
            }
            this.totalPriceSetup();
        },
        totalPriceSetup: function () {
            this.temp.totalPrice = this.temp.price * this.temp.quantity;
        },
        addToCart: function () {
            this.enableAddToCardButton = true;
            this.productArray = {
                name: this.temp.name,
                product_id: this.temp.productId,
                image: this.temp.image,
                variation_names: "",
                variation_id: this.temp.variationId,
                sku: this.temp.sku,
                stock: this.temp.stock,
                taxes: this.temp.taxes,
                shipping: this.temp.shipping,
                quantity: this.temp.quantity,
                discount: this.temp.discount,
                price: this.temp.price,
                old_price: this.temp.oldPrice,
                total_price: this.temp.totalPrice,
            };

            if (this.selectedVariation) {
                this.$store
                    .dispatch(
                        "frontendProductVariation/ancestorsToString",
                        this.selectedVariation.id
                    )
                    .then((res) => {
                        this.productArray.variation_names = res.data.data;
                        this.variationComponent = false;
                        this.$store
                            .dispatch("frontendCart/lists", this.productArray)
                            .then((res) => {
                                alertService.success(
                                    this.$t("message.add_to_cart")
                                );
                                this.variationComponent = true;
                                this.productArray = {};
                                this.selectedVariation = null;
                                this.temp.isVariation =
                                    this.initProduct.isVariation;
                                this.temp.variationId =
                                    this.initProduct.variationId;
                                this.temp.sku = this.initProduct.sku;
                                this.temp.stock = this.initProduct.stock;
                                this.temp.quantity = this.initProduct.quantity;
                                this.temp.discount = this.initProduct.discount;
                                this.temp.price = this.initProduct.price;
                                this.temp.oldPrice = this.initProduct.oldPrice;
                                this.temp.totalPrice = this.initProduct.price;
                            })
                            .catch((err) => {
                                alertService.error(
                                    this.$t("message.maximum_quantity")
                                );
                                this.variationComponent = true;
                                this.selectedVariation = null;
                                this.temp.stock = this.initProduct.stock;
                                this.temp.quantity = this.initProduct.quantity;
                            });
                    })
                    .catch((err) => {});
            } else {
                this.$store
                    .dispatch("frontendCart/lists", this.productArray)
                    .then((res) => {
                        alertService.success(this.$t("message.add_to_cart"));
                        this.enableAddToCardButton = false;
                        this.productArray = {};
                        this.selectedVariation = null;
                        this.temp.isVariation = this.initProduct.isVariation;
                        this.temp.variationId = this.initProduct.variationId;
                        this.temp.sku = this.initProduct.sku;
                        this.temp.stock = this.initProduct.stock;
                        this.temp.quantity = this.initProduct.quantity;
                        this.temp.discount = this.initProduct.discount;
                        this.temp.price = this.initProduct.price;
                        this.temp.oldPrice = this.initProduct.oldPrice;
                        this.temp.totalPrice = this.initProduct.price;
                    })
                    .catch((err) => {
                        alertService.error(this.$t("message.maximum_quantity"));
                        this.enableAddToCardButton = false;
                        this.selectedVariation = null;
                        this.temp.stock = this.initProduct.stock;
                        this.temp.quantity = this.initProduct.quantity;
                    });
            }
        },
    },
    watch: {
        $route() {
            this.show();
            this.showRelatedProduct();
        },
    },
};
</script>
