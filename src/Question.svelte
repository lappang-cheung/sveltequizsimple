<script>
    export let question, 
               nextQuestion,
               addToScore 

    let answers = question.incorrect_answers
        .map(answer => {
            return {
                answer,
                correct: false
            }
        }),

        allAnswers = [
            ...answers, 
            {
                answer: question.correct_answer,
                correct: true
            }
        ],
        isCorrect,
        isAnswered = false

    const shuffle = (array) => {
        array.sort(() => Math.random() - 0.5)
    }

    const checkQuestion = (correct) => {
        if(!isAnswered) {
            isAnswered = true
            isCorrect = correct

            if(correct) {
                addToScore()
            }
        }
    }

    shuffle(allAnswers)

</script>

<h3>
    {@html question.question}
</h3>
{#if isAnswered}
    <h5>
        {#if isCorrect }
            You got it right
        {:else}
            You goofed
        {/if}
    </h5>
{/if}

{#each allAnswers as answer}
    <button on:click={() => checkQuestion(answer.correct)}>
        {@html answer.answer}
    </button>
{/each}
{#if isAnswered}
    <div>
        <button on:click={nextQuestion}>Next Question</button>
    </div>
{/if}