<script setup>
import DangerButton from '@/Components/DangerButton.vue';
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
import Swal from 'sweetalert2';

const props = defineProps({
    // Con esto llamo los datos que vienen del contralador
    dependencies: { type: Object }
});

const form = useForm({
    id: ''
})

const deleteDependencies = (id, name) => {
    const alerta = Swal.mixin({
        buttonsStyling: true
    });

    alerta.fire({
        title: 'Seguro de eliminar ' + name + ' ?',
        icon: 'question', showCancelButton: true,
        confirmButtonText: '<i class="fa-solid fa-check"></i> Si, eliminar',
        cancelButtonText: '<i class="fa-solid fa-ban"></i> Cancelar'
    }).then((result) => {
        if (result.isConfirmed) {
            form.delete(route('dependencies.destroy', id));
        }

    });
}

</script>

<template>
    <Head title="Dependencias" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="text-xl font-semibold leading-tight text-gray-800">Dependencias</h2>
        </template>

        <div class="py-12 mb-4">
            <div class="grid bg-white v-screen place-items-center">
                <div class="flex mt-3 mb-3">
                    <Link :href="route('dependencies.create')"
                        :class="'px-4 py-2 bg-gray-800 text-white border rounded-md font-semibold text-xs'">
                    <i class="fa-solid fa-plus-circle"></i> Agregar
                    </Link>
                </div>
            </div>
            <div class="grid bg-white v-screen place-items-center">
                <table class="border border-gray-400 table-auto">
                    <thead>
                        <tr class="bg-gray-100">
                            <th class="px-4 py-4">#</th>
                            <th class="px-4 py-4">Dependencias</th>
                            <th class="px-4 py-4"></th>
                            <th class="px-4 py-4"></th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="dep, i in dependencies" :key="dep.id">
                            <td class="px-4 py-4 border border-gray-400">{{ (i + 1) }}</td>
                            <td class="px-4 py-4 border border-gray-400">{{ (dep.name) }}</td>
                            <td class="px-4 py-4 border border-gray-400">
                                <Link :href="route('dependencies.edit',dep.id)"
                                    :class="'px-2 py-2 bg-yellow-400 text-gray border rounded-md font-semibold text-xs'">
                                <i class="fa-solid fa-edit"></i>
                                </Link>
                            </td>
                            <td class="px-4 py-4 border border-gray-400">
                                <DangerButton @click="$event=>deleteDependencies(dep.id,dep.name)">
                                    <i class="fa-solid fa-trash"></i>
                                </DangerButton>
                            </td>
                            
                        </tr>
                    </tbody>

                </table>

            </div>

        </div>
    </AuthenticatedLayout>
</template>
