<template>
    <div class="py-6 lg:rounded-2xl lg:shadow-card bg-white border border-[#D9DBE9]">
        <div class="flex flex-col items-center justify-center mb-5">
            <RouterLink to="#" class="w-20 h-20 mb-3 rounded-full border border-primary">
                <img :src="image" alt="avatar" class=" w-full h-full object-cover rounded-full border-2 border-white" loading="lazy">
            </RouterLink>
            <h3 class="capitalize text-lg font-semibold text-center mb-0.5">{{ name }}</h3>
            <p class="text-center text-text">{{ phone }}</p>
        </div>
        <nav class="flex flex-col">
            <router-link
                class="profile-link font-medium flex items-center gap-4 capitalize py-3 px-4 group hover:text-primary transition-all duration-500"
                :to="{ name: 'frontend.account.overview' }">
                <img class="account-icn group-hover:text-primary transition-all duration-500" src="public/images/default/icon/grid.svg">
                <span>{{ $t('menu.overview') }}</span>
            </router-link>

            <router-link
                class="profile-link font-medium flex items-center gap-4 capitalize py-3 px-4 group hover:text-primary transition-all duration-500"
                :to="{ name: 'frontend.account.orderHistory' }">
                <img class="account-icn group-hover:text-primary transition-all duration-500" src="public/images/default/icon/invite-alt.svg">
                <span>{{ $t('menu.order_history') }}</span>
            </router-link>

            <router-link
                class="profile-link font-medium flex items-center gap-4 capitalize py-3 px-4 group hover:text-primary transition-all duration-500"
                :to="{ name: 'frontend.account.returnOrders' }">
                <img class="account-icn group-hover:text-primary transition-all duration-500" src="public/images/default/icon/undo.svg">
                <span>{{ $t('menu.return_orders') }}</span>
            </router-link>

            <router-link
                class="profile-link font-medium flex items-center gap-4 capitalize py-3 px-4 group hover:text-primary transition-all duration-500"
                :to="{ name: 'frontend.account.accountInfo' }">
                <img class="account-icn group-hover:text-primary transition-all duration-500" src="public/images/default/icon/circle-user.svg">
                <span>{{ $t('menu.account_info') }}</span>
            </router-link>

            <router-link
                class="profile-link font-medium flex items-center gap-4 capitalize py-3 px-4 group hover:text-primary transition-all duration-500"
                :to="{ name: 'frontend.account.changePassword' }">
                <img class="account-icn group-hover:text-primary transition-all duration-500" src="public/images/default/icon/password.svg">
                <span>{{ $t('menu.change_password') }}</span>
            </router-link>

            <router-link
                class="profile-link font-medium flex items-center gap-4 capitalize py-3 px-4 group hover:text-primary transition-all duration-500"
                :to="{ name: 'frontend.account.address' }">
                <img class="account-icn group-hover:text-primary transition-all duration-500" src="public/images/default/icon/address-book.svg">
                <span>{{ $t('menu.address') }}</span>
            </router-link>
        </nav>
    </div>
</template>

<script>
export default {
    name: "AccountMenuComponent",
    data() {
        return {
            image: "",
            name: "",
            phone: "",
        };
    },
    mounted() {
        try {
            const profile = this.$store.getters.authInfo;
            this.image = profile.image;
            this.name = profile.name;
            this.phone = profile.phone ? profile.country_code + profile.phone : '';
        } catch (err) {
            alertService.error(err);
        }
    },
};
</script>
