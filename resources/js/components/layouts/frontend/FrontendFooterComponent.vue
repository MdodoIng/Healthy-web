<template>
    <LoadingComponent :props="loading" />

    <footer class="pt-12 bg-clr mobile:hidden">
        <div class="container">
            <div
                style="gap: 80px; justify-content: space-between"
                class="row lg:!flex-nowrap flex-wrap"
            >
                <div class="col-12 md:col-4 lg:col-3 mb-6 md:mb-0">
                    <div
                        class="tablet:text-center tablet:mx-auto w-full max-w-xs"
                    >
                        <router-link :to="{ name: 'frontend.home' }">
                            <img
                                class="pb-5 w-13 lgo_top"
                                :src="setting.theme_footer_logo"
                                alt="logo"
                                loading="lazy"
                            />
                        </router-link>

                        <ul class="flex flex-col gap5 pt-5">
                            <!-- <li class="flex gap-3">
                                <img
                                    class="icon_smedia"
                                    src="public/images/default/icon/marker.svg"
                                />
                                <span
                                    class="adds_tx font-medium text-black -mt-1"
                                    >{{ setting.company_address }}</span
                                >
                            </li> -->
                            <li class="flex gap-3">
                                <!--<i class="lab-fill-mail text-sm flex-shrink-0 text-white"></i>-->
                                <img
                                    class="icon_smedia"
                                    src="public/images/default/icon/envelope-dot.svg"
                                />
                                <span
                                    class="text-ftr font-medium text-black -mt-1"
                                    >{{ setting.company_email }}</span
                                >
                            </li>
                            <li class="flex gap-3">
                                <img
                                    class="icon_smedia"
                                    src="public/images/default/icon/square-phone-hangup.svg"
                                />
                                <span
                                    class="text-ftr font-medium !text-[22px] text-black -mt-1"
                                    >{{ setting.company_phone }}</span
                                >
                            </li>
                        </ul>
                        <div
                            style="max-width: 260px"
                            class="col-6 sm:col-4 mt-5"
                        >
                            <!-- <h4
                                class="text-[22px] font-semibold capitalize mb-6 text-black ftr-titl"
                            >
                                {{ $t("label.follow_us") }}
                            </h4> -->
                            <nav
                                v-if="
                                    setting.social_media_facebook ||
                                    setting.social_media_twitter ||
                                    setting.social_media_instagram ||
                                    setting.social_media_youtube
                                "
                                class="flex flex-wrap items-center gap-6 tablet:justify-start"
                            >
                                <a
                                    v-if="setting.social_media_facebook"
                                    target="_blank"
                                    :href="setting.social_media_facebook"
                                    class="lab-fill-facebook w-7 h-7 !leading-7 text-center rounded-full text-sm text-secondary bg-white transition-all duration-300 hover:text-white hover:bg-primary"
                                ></a>
                                <a
                                    v-if="setting.social_media_twitter"
                                    target="_blank"
                                    :href="setting.social_media_twitter"
                                    class="lab-fill-x w-7 h-7 !leading-7 text-center rounded-full text-sm text-secondary bg-white transition-all duration-300 hover:text-white hover:bg-primary"
                                ></a>
                                <a
                                    v-if="setting.social_media_instagram"
                                    target="_blank"
                                    :href="setting.social_media_instagram"
                                    class="lab-fill-instagram w-7 h-7 !leading-7 text-center rounded-full text-sm text-secondary bg-white transition-all duration-300 hover:text-white hover:bg-primary"
                                ></a>
                                <a
                                    v-if="setting.social_media_youtube"
                                    target="_blank"
                                    :href="setting.social_media_youtube"
                                    class="lab-fill-youtube w-7 h-7 !leading-7 text-center rounded-full text-sm text-secondary bg-white transition-all duration-300 hover:text-white hover:bg-primary"
                                ></a>
                            </nav>
                            <div v-else class="fb-iner">
                                <div class="footer-social">
                                    <ul>
                                        <li>
                                            <a href="#"
                                                ><img
                                                    class="icon_ftr"
                                                    src="public/images/default/icon/facebook.svg"
                                            /></a>
                                        </li>
                                        <li>
                                            <a href="#"
                                                ><img
                                                    class="icon_ftr"
                                                    src="public/images/default/icon/instagram.svg"
                                            /></a>
                                        </li>
                                    </ul>
                                </div>
                            </div>
                            <!-- <span
                                class="my-5 h-[1px] w-full bg-[#016d6d12]"
                            ></span>
                            <form
                                @submit.prevent="saveSubscription"
                                class="mb-6 block"
                            >
                                <label class="mb-3 font-medium text-black">
                                    {{
                                        $t(
                                            "message.subscribe_to_our_newsletter"
                                        )
                                    }}
                                </label>
                                <div
                                    class="flex w-full h-10 radius-10 p-1 bg-white mt-3 border border-[#89898952]"
                                >
                                    <input
                                        type="email"
                                        v-model="subscriptionProps.post.email"
                                        :placeholder="
                                            $t('label.your_email_address')
                                        "
                                        class="w-full h-full pl-3 pr-2"
                                    />
                                    <button
                                        type="submit"
                                        class="text-xs font-semibold capitalize flex-shrink-0 px-3 h-full radius-10 bg-primary text-black"
                                    >
                                        {{ $t("button.subscribe") }}
                                    </button>
                                </div>
                            </form> -->
                        </div>
                    </div>
                </div>
                <div class="col-12 md:col-8 lg:col-8 lg:!flex-nowrap">
                    <div
                        style="justify-content: flex-end; gap: 40px"
                        class="row lg:!flex-nowrap"
                    >
                        <div style="max-width: 260px" class="col-12 sm:col-4">
                            <div class="flex flex-col">
                                <h4
                                    class="text-[22px] font-semibold capitalize mb-6 text-black ftr-titl"
                                >
                                    <!-- {{ $t("label.useful_links") }} -->
                                    shop 1
                                </h4>
                                <span
                                    class="adds_tx font-medium text-black -mt-1"
                                    >{{ setting.company_address }}</span
                                >
                            </div>
                            <div class="flex flex-col mt-6">
                                <h4
                                    class="text-[22px] font-semibold capitalize mb-6 text-black ftr-titl"
                                >
                                    <!-- {{ $t("label.useful_links") }} -->
                                    shop 2
                                </h4>
                                <span
                                    class="adds_tx font-medium text-black -mt-1"
                                    >{{ setting.company_address }}</span
                                >
                            </div>
                        </div>

                        <!--  -->
                        <div style="max-width: 260px" class="col-12 sm:col-4">
                            <h4
                                class="text-[22px] font-semibold capitalize mb-6 text-black ftr-titl"
                            >
                                {{ $t("label.useful_links") }}
                            </h4>
                            <nav
                                v-if="legalPages.length > 0"
                                class="flex flex-col gap-4 pt-5"
                            >
                                <router-link
                                    v-for="legalPage in legalPages"
                                    class="w-fit text-ftr font-medium capitalize text-black transition-all duration-300 hover:text-primary"
                                    :to="{
                                        name: 'frontend.page',
                                        params: { slug: legalPage.slug },
                                    }"
                                >
                                    {{ legalPage.title }}
                                </router-link>
                            </nav>

                            <dl class="mt-6">
                                <dd class="flex gap-3">
                                    <a
                                        target="_blank"
                                        class="router-link-active router-link-exact-active"
                                        v-if="setting.site_android_app_link"
                                        :href="setting.site_android_app_link"
                                    >
                                        <img
                                            class="h-8 rounded-lg"
                                            :src="setting.image_play_store"
                                            alt="app"
                                            loading="lazy"
                                        />
                                    </a>
                                    <a
                                        target="_blank"
                                        class="router-link-active router-link-exact-active"
                                        v-if="setting.site_ios_app_link"
                                        :href="setting.site_ios_app_link"
                                    >
                                        <img
                                            class="h-8 rounded-lg"
                                            :src="setting.image_app_store"
                                            alt="app"
                                            loading="lazy"
                                        />
                                    </a>
                                </dd>
                            </dl>
                        </div>

                        <div
                            style="max-width: 260px"
                            class="col-6 sm:col-4 mb-4 sm:mb-0"
                        >
                            <a
                                href=""
                                target="_blank"
                                class="flex items-center aspect-square justify-center w-full rounded-[10px] overflow-hidden"
                            >
                                <img
                                    src="public/images/required/footer location.jpg"
                                    class="w-full h-full object-cover"
                                />
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="pt-4 pb-24 lg:py-4 mt-8 text-center copy_bg">
            <p class="footer-xs font-medium text-white">
                {{ setting.site_copyright }}
            </p>
        </div>
    </footer>
</template>

<script>
import statusEnum from "../../../enums/modules/statusEnum";
import axios from "axios";
import alertService from "../../../services/alertService";
import LoadingComponent from "../../frontend/components/LoadingComponent";
import menuSectionEnum from "../../../enums/modules/menuSectionEnum";
import "../../../../css/app.css";
import _ from "lodash";

export default {
    name: "FrontendFooterComponent",
    components: { LoadingComponent },
    data() {
        return {
            loading: {
                isActive: false,
            },
            legalPages: [],
            supportPages: [],
            enums: {
                statusEnum: statusEnum,
                menuSectionEnum: menuSectionEnum,
            },
            subscriptionProps: {
                post: {
                    email: "",
                },
            },
        };
    },
    computed: {
        setting: function () {
            return this.$store.getters["frontendSetting/lists"];
        },
    },
    mounted() {
        this.loading.isActive = true;
        this.$store
            .dispatch("frontendPage/lists", {
                paginate: 0,
                order_column: "id",
                order_type: "asc",
                status: this.enums.statusEnum.ACTIVE,
            })
            .then((res) => {
                if (res.data.data.length > 0) {
                    _.forEach(res.data.data, (page) => {
                        if (
                            page.menu_section_id ===
                            this.enums.menuSectionEnum.LEGAL
                        ) {
                            this.legalPages.push(page);
                        } else {
                            this.supportPages.push(page);
                        }
                    });
                }
                this.loading.isActive = false;
            })
            .catch((err) => {
                this.loading.isActive = false;
            });
    },
    methods: {
        saveSubscription: function () {
            const url = "/frontend/subscriber";
            this.loading.isActive = true;
            axios
                .post(url, this.subscriptionProps.post)
                .then((res) => {
                    this.loading.isActive = false;
                    this.subscriptionProps.post.email = "";
                    alertService.success(this.$t("message.subscribe"));
                })
                .catch((err) => {
                    this.loading.isActive = false;
                });
        },
    },
};
</script>
