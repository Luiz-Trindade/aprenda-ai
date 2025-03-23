<template>
    <v-container>
        <v-card elevation="4">
            <h4 style="display: flex; padding: 10px; text-align: center;">
                {{ question }}
            </h4>
            
            <v-card-actions class="flex flex-column">
                <v-btn 
                    elevation   = "4" 
                    color       = "auto"
                    variant     = "tonal" 
                    v-for       = "(option, index) in options" 
                    :key        = "index" 
                    @click      = "selectAnswer(option)"
                    :class      = "{'selected': answer === option}"
                    width       = "100%"
                    height      = "auto"
                    class       = "word-wrap-btn"
                >
                    <div class="button-text">
                        {{ option }}
                    </div>
                </v-btn>
            </v-card-actions>

            <v-alert v-if="isAnswered" :type="isCorrect ? 'success' : 'error'" class="mt-4">
                {{ isCorrect ? 'Resposta Correta!' : 'Resposta Errada, tente novamente.' }}
            </v-alert>
        </v-card>
    </v-container>
</template>
  
<script>
export default {
    name: "QuizCard",
    props: {
        question: {
            type: String,
            required: true,
        },
        options: {
            type: Array,
            required: true,
        },
        correctAnswer: {
            type: String,
            required: true,
        },
    },
    data() {
        return {
            answer      : "",
            isAnswered  : false,
            isCorrect   : false,
        };
    },

    methods: {
        selectAnswer(answer) {
            this.answer     = answer;
            this.isAnswered = true;
            this.isCorrect  = this.answer === this.correctAnswer;
        },
    },
};
</script>
  
<style scoped>
.selected {
  background-color: #007bff; /* Cor de fundo para o botão selecionado */
  color: white; /* Cor do texto do botão selecionado */
}
.word-wrap-btn {
  display: flex;          /* Usar flexbox para controlar o layout */
  justify-content: center; /* Centraliza o conteúdo */
  align-items: center;     /* Garante que o texto seja centralizado verticalmente */
  min-height: 60px;        /* Defina a altura mínima do botão para dar espaçamento */
  text-align: center;      /* Centraliza o texto no botão */
  margin-bottom: 10px;     /* Distância entre os botões */
  padding: 10px;
}

.button-text {
  word-wrap: break-word;    /* Permite quebra de linha nas palavras longas */
  white-space: normal;      /* Quebra as linhas quando necessário */
  text-align: center;       /* Centraliza o texto dentro do div */
  margin: 0;                /* Remove qualquer margem extra */
}
</style>
  