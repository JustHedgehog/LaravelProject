<template>
    <div v-if="isError" class="modal">
        <div class="modal-content">
            <h3>Upps, coś poszło nie tak!</h3>
            <h6 class="text-justify">{{ message }}</h6>
            <div class="d-flex mt-3">
                <button @click.prevent="hide" class="btn btn-light mr-3">Ukryj</button>
                <a href="/" class="btn btn-light">Strona główna</a>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    inject: ["eventBus"],
    created() { this.eventBus.$on("error", this.handleError); },
    beforeDestroy() { this.eventBus.$off("error"); },
    data() {
        return {
            isError: false,
            message: ""
        };
    },
    methods: {
        hide() { this.isError = false; },
        handleError(err) {
            this.isError = true;
            if (err.response) {
                this.message = `${err.response.statusText} - ${err.response.status}`;
            } else {
                this.message = "HTTP Error happend!";
            }
            console.log(err);
        }
    },
    watch: {
        $route(to, from) { this.hide(); }
    }
};
</script>
<style scoped>
.modal {
    display: flex;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0, 0, 0);
    background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
    background-color: #fefefe;
    align-items: center;
    margin: 20% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 60%;
    height: 20%;
}
</style>