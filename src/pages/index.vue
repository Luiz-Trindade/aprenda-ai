<template>
  <!-- <HelloWorld /> -->
  <QuizCard 
    :question="question" 
    :options="options"
    :correctAnswer="correctAnswer"
  />
</template>

<script>

export default {
  name: "index",

  data() {
    return {
      question      : "",
      options       : [],
      correctAnswer : "",
      topics_prompt : "gere tópicos sobre o seguinte tema 'Python para automação e scripts' em um array javascript apenas. exemplo: ['tópico1', 'tópico2', 'tópico2']",
      topics_list   : [],
    }
  },

  mounted() {
    this.get_quiz_question();
    this.use_llm(this.topics_prompt);
  },

  methods: {
    use_llm(prompt) {
      const apiKey      = 'AIzaSyDyMncoXz4JNoJV2bRz1Df1YCoxpHRvc30';
      const model       = "gemini-2.0-flash"
      const url         = `https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent?key=${apiKey}`;

      const data = {
        contents: [{
          parts: [{
            text: prompt
          }]
        }]
      };

      this.$axios.post(url, data, {
        headers: {
          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          let contentText = response.data.candidates[0].content.parts[0].text;
          console.log(contentText);

          // Remove os backticks de marcação do JSON
          contentText = contentText.replace(/```(?:json|[a-zA-Z]+)?\n([\s\S]*?)\n```/g, "$1");

          // Agora pode parsear o JSON corretamente
          try {
            const json_response = JSON.parse(contentText);
            console.log(json_response);
            this.topics_list = json_response;
          } catch (error) {
            console.error("Erro ao fazer parse do JSON:", error);
            this.use_llm()
          }
        })
        .catch(error => {
          console.error('Erro:', error.response ? error.response.data : error.message);
        });
    },

    get_quiz_question() {
      const apiKey      = 'AIzaSyDyMncoXz4JNoJV2bRz1Df1YCoxpHRvc30';
      const model       = "gemini-2.0-flash"
      const url         = `https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent?key=${apiKey}`;
      const tema        = "Português para concursos";
      const dificuldade = "Médio";

      const data = {
        contents: [{
          parts: [{
            text: `
              Você é um criador de quiz profissional.
              Crie um quiz variado de dificuldade "${dificuldade}" sobre "${tema}" com 1 pergunta, 
              4 opções e a resposta correta. O formato de saída deve ser apena um JSON com 
              as propriedades 'question', 'options' e 'correctAnswer'.
              
              Exemplo de quiz
              #####
              {
                'question'      : 'Qual é a capital do Brasil?',
                'options'       : ['Brasília', 'São Paulo', 'Rio de Janeito', 'Salvador'],
                'correctAnswer' : 'Brasília'
              }
              #####
              `
          }]
        }]
      };

      this.$axios.post(url, data, {
        headers: {
          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          let contentText = response.data.candidates[0].content.parts[0].text;
          console.log(contentText);

          // Remove os backticks de marcação do JSON
          // contentText = contentText.replace(/^`{3}json|`{3}$/g, "");
          contentText = contentText.replace(/```(?:json|[a-zA-Z]+)?\n([\s\S]*?)\n```/g, "$1");


          // Agora pode parsear o JSON corretamente
          try {
            const json_response = JSON.parse(contentText);
            
            this.question       = json_response.question;
            this.options        = json_response.options;
            this.correctAnswer  = json_response.correctAnswer;

            console.log(json_response); // Log dos dados do quiz

          } catch (error) {
            console.error("Erro ao fazer parse do JSON:", error);
            this.get_quiz_question()
          }
        })
        .catch(error => {
          console.error('Erro:', error.response ? error.response.data : error.message);
        });
    }
  }
}
</script>
