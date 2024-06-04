<template>
    <LoadingComponent :props="loading" />
    <section
        v-if="benefits.length > 0"
        class="pt-8 pb-24 sm:py-12 border-t border-slate-100"
    >
        <div class="container">
            <div class="grid grid-cols-2 md:grid-cols-3 gap-5 del-cln">
                <div
                    v-for="benefit in benefits"
                    class="w-full max-w-[350px] relative lg:pl-9 del-bx"
                >
                    <img
                        :src="benefit.cover"
                        alt="benefit"
                        width="60"
                        height="60"
                        class="w-9 mb-4 lg:mb-0 lg:absolute lg:top-0 lg:left-0 object-contain"
                        loading="lazy"
                    />
                    <h4
                        class="text-base font-semibold capitalize mb-2 benef_tt text-white"
                    >
                        {{ benefit.title }}
                    </h4>
                    <p class="text-sm benef_tt text-white">
                        {{ benefit.description }}
                    </p>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import statusEnum from "../../../enums/modules/statusEnum";
import LoadingComponent from "../components/LoadingComponent";

export default {
    name: "BenefitComponent",
    components: {
        LoadingComponent,
    },
    data() {
        return {
            loading: {
                isActive: false,
            },
        };
    },
    computed: {
        benefits: function () {
            return this.$store.getters["frontendBenefit/lists"];
        },
    },
    mounted() {
        this.loading.isActive = true;
        this.$store
            .dispatch("frontendBenefit/lists", {
                paginate: 0,
                order_column: "id",
                order_type: "asc",
                status: statusEnum.ACTIVE,
            })
            .then((res) => {
                this.loading.isActive = false;
            })
            .catch((err) => {
                this.loading.isActive = false;
            });
    },
};
</script>
