<template>
    <Head>
        <title>Edit Role - Aplikasi Kasir</title>
    </Head>
    <main class="c-main">
        <div class="container-fluid">
            <div class="fade-in">
                <div class="row">
                    <div class="col-md-12">
                        <div
                            class="card border-0 rounded-3 shadow border-top-purple"
                        >
                            <div class="card-header">
                                <span class="font-weight-bold"
                                    ><i class="fa fa-shield-alt"></i> EDIT
                                    ROLE</span
                                >
                            </div>
                            <div class="card-body">
                                <form @submit.prevent="submit">
                                    <div class="mb-3">
                                        <label class="fw-bold">Role Name</label>
                                        <input
                                            class="form-control"
                                            v-model="form.name"
                                            :class="{
                                                'is-invalid': errors.name,
                                            }"
                                            type="text"
                                            placeholder="Role Name"
                                        />

                                        <div
                                            v-if="errors.name"
                                            class="alert alert-danger"
                                        >
                                            {{ errors.name }}
                                        </div>
                                    </div>

                                    <hr />
                                    <div class="mb-3">
                                        <label class="fw-bold"
                                            >Permissions</label
                                        >
                                        <br />
                                        <div
                                            class="col-md-12"
                                            v-for="(
                                                permissionGroup, key
                                            ) in permissions"
                                            :key="key"
                                        >
                                            <div class="col-md mb-4 mb-md-2">
                                                <div
                                                    class="accordion mt-3"
                                                    :id="key"
                                                >
                                                    <div
                                                        class="card accordion-item"
                                                    >
                                                        <h2
                                                            class="accordion-header"
                                                            :id="
                                                                'heading' + key
                                                            "
                                                        >
                                                            <button
                                                                type="button"
                                                                class="accordion-button collapsed"
                                                                data-bs-toggle="collapse"
                                                                :data-bs-target="
                                                                    '#accordion' +
                                                                    key
                                                                "
                                                                aria-expanded="false"
                                                                :aria-controls="
                                                                    'accordion' +
                                                                    key
                                                                "
                                                            >
                                                                <b>{{ key }}</b>
                                                            </button>
                                                        </h2>

                                                        <div
                                                            :id="
                                                                'accordion' +
                                                                key
                                                            "
                                                            class="accordion-collapse collapse show"
                                                            :data-bs-parent="
                                                                '#' + key
                                                            "
                                                        >
                                                            <div
                                                                class="accordion-body"
                                                            >
                                                                <div
                                                                    class="form-check form-check-inline"
                                                                    v-for="(
                                                                        permission,
                                                                        index
                                                                    ) in permissionGroup"
                                                                    :key="index"
                                                                >
                                                                    <input
                                                                        class="form-check-input"
                                                                        type="checkbox"
                                                                        v-model="
                                                                            form.permissions
                                                                        "
                                                                        :value="
                                                                            permission
                                                                        "
                                                                        :id="`check-${key}-${index}`"
                                                                    />
                                                                    <label
                                                                        class="form-check-label"
                                                                        :for="`check-${key}-${index}`"
                                                                    >
                                                                        {{
                                                                            permission
                                                                        }}
                                                                    </label>
                                                                </div>
                                                            </div>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>

                                    <div class="row">
                                        <div class="col-12">
                                            <button
                                                class="btn btn-primary shadow-sm rounded-sm"
                                                type="submit"
                                                :disabled="form.isSubmitting"
                                            >
                                                {{
                                                    form.isSubmitting
                                                        ? "Processing..."
                                                        : "Update"
                                                }}
                                            </button>
                                            <button
                                                class="btn btn-warning shadow-sm rounded-sm ms-3"
                                                type="reset"
                                            >
                                                RESET
                                            </button>
                                        </div>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>

<script>
import LayoutApp from "../../../Layouts/App.vue";

//import Heade and useForm from Inertia
import { Head, Link } from "@inertiajs/inertia-vue3";

//import reactive from vue
import { reactive } from "vue";

//import inerita adapter
import { Inertia } from "@inertiajs/inertia";

//import sweet alert2
import Swal from "sweetalert2";

export default {
    //layout
    layout: LayoutApp,

    //register component
    components: {
        Head,
        Link,
    },

    //props
    props: {
        errors: Object,
        permissions: Array,
        role: Object,
    },

    //composition API
    setup(props) {
        //define form with reactive
        const form = reactive({
            name: props.role.name,
            isSubmitting: false,
            permissions: props.role.permissions.map((obj) => obj.name),
        });

        //method "submit"
        const submit = () => {
            //send data to server
            form.isSubmitting = true;

            Inertia.put(
                `/apps/roles/${props.role.id}`,
                {
                    //data
                    name: form.name,
                    permissions: form.permissions,
                },
                {
                    onSuccess: () => {
                        //show success alert
                        Swal.fire({
                            title: "Success!",
                            text: "Role updated successfully.",
                            icon: "success",
                            showConfirmButton: false,
                            timer: 2000,
                        });
                    },
                    onError: () => {
                        form.isSubmitting = false;
                    },
                }
            );
        };

        return {
            form,
            submit,
        };
    },
};
</script>

<style></style>
