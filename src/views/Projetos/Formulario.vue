<template>
    <section>
        <form @submit.prevent="salvar">
            <div class="field">
                <label class="label" for="nomeDoProjeto">Nome Do Projeto</label>
                <input type="text" class="input" v-model="nomeDoProjeto" id="nomeDoprojet">
            </div>
            <div class="field">
                <button class="button" type="submit">Salvar</button>
            </div>
        </form>
    </section>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { useStore } from '@/store';
import { ALTERAR_PROJETO } from '@/store/tipo-acoes';
import { TipoNotificacao } from '@/interfaces/INotificacao';
'@vue/runtime-core';

import useNotificador from '@/hooks/notificador'
import { CADASTRAR_PROJETOS } from '@/store/tipo-acoes';
import { useRouter } from 'vue-router';

export default defineComponent({
    name: 'FormularioVue',
    props: {
        id: {
            type: String
        }
    },
    // mounted() {
    //     if (this.id) {
    //         const projeto = this.store.state.projeto.projetos.find(prj => prj.id == this.id)
    //         this.nomeDoProjeto = projeto?.nome || ''
    //     }
    // },
    // data() {
    //     return {
    //         nomeDoProjeto: ''
    //     };
    // },
    setup(props) {
        const store = useStore()
        const router = useRouter()
        const { notificar } = useNotificador()

        const nomeDoProjeto = ref("")

        if (props.id) {
            const projeto = store.state.projeto.projetos.find(prj => prj.id == props.id)
            nomeDoProjeto.value = projeto?.nome || ''
        }

        const casoSucesso = () => {
            nomeDoProjeto.value = ''
            notificar(TipoNotificacao.ATENCAO, 'Sucesso', 'Projeto Notificado Com Sucesso')
            router.push('/projetos')
        }

        const salvar = () => {
            if (props.id) {
                store.dispatch(ALTERAR_PROJETO, {
                    id: props.id,
                    nome: nomeDoProjeto.value
                }).then(() => casoSucesso())
            } else {
                store.dispatch(CADASTRAR_PROJETOS, nomeDoProjeto.value)
                    .then(() => casoSucesso())
            }

        }
        
        return {
            nomeDoProjeto,
            salvar
        }
    }
})
</script>