<template>
    <div>
        Dashboard component
        <div v-if="user">Zalogowany użytkownik: {{ user }} </div>
    </div>
</template>
<script>
export default {
    inject: ["auth"],
    data() {
        return {
            user: null
        };
    },
    created() {
        this.getUser();
    },
    methods: {
        async getUser() {
            let user = await this.auth.getAuthUser();
            this.user = user.data.name;
        }
    }
};
</script>