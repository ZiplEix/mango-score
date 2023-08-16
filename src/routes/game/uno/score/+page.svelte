<script lang="ts">
    import { onMount } from "svelte";
    import { writable } from "svelte/store";

    const nbPlayer = localStorage.getItem("nbPlayers");
    const nbPointToStop = localStorage.getItem("nbPointToStop");
    const playersName = ["#", ...JSON.parse(localStorage.getItem("playersName"))];

    // let gridData = [playersName];
    const gridData = writable([playersName]);
    const totalScore = ["Total", ...Array(Number(nbPlayer)).fill(0)]

    const handleSubmit = () => {
        const newScores: number[] = [];
        const scoreInputs = document.querySelectorAll('input[name="score"]');
        scoreInputs.forEach((input) => {
            const newScore = Number(input.value);
            newScores.push(newScore);
        });
        gridData.update((gridData) => [...gridData, newScores]);

        // update the total score
        totalScore.forEach((score, index) => {
            if (index > 0) {
                totalScore[index] = Number(score) + newScores[index - 1];
            }
        });

        // reset the input
        scoreInputs.forEach((input) => {
            input.value = "0";
        });

        // scroll to the bottom
        const main = document.querySelector("main");
        main.scrollTop = main.scrollHeight;

        // check if the game is finished
        if (nbPointToStop !== "-1") {
            const isFinished = totalScore.some((score, index) => {
                if (index > 0) {
                    return score >= Number(nbPointToStop);
                }
            });
            if (isFinished) {
                alert("Game finished");
            }
        }
    };

    onMount(() => {
        document.querySelector("button")?.addEventListener("click", handleSubmit);
    });

</script>

<main>
    <h1>Uno Score Keeper</h1>

    <!-- display the usernames -->
    <div class="row bold">
        {#each $gridData[0] as playerName, index}
            <div class="cell">{playerName}</div>
        {/each}
    </div>

    <!-- display the grid -->
    {#each $gridData as row, index}
        {#if index !== 0}
            <div class="row">
                {#if index > 0}
                    <div class="cell bold">{index}</div>
                {/if}
                {#each row as cell, j}
                    <div class="cell">{cell}</div>
                {/each}
            </div>
        {/if}
    {/each}

    <!-- display the total score -->
    <div class="row total">
        {#each totalScore as score}
            <div class="cell">{score}</div>
        {/each}
    </div>

    <!-- allow to add score -->
    <div class="row">
        {#each $gridData[0] as playerName, index}
            {#if index > 0}
                <input type="text" name="score" class="newScore" value={0} />
            {/if}
            {#if index === 0}
                <div class="newScore">Enter score</div>
            {/if}
        {/each}
    </div>
    <button>Valider</button>
</main>

<style>
    :global(body) {
        margin: 0;
        padding: 0;

        background-color: #fdfcfa;

        /* display the content centered verticaly */
        display: flex;
        flex-direction: column;

        font-family: Arial, Helvetica, sans-serif;
    }

    main {
        width: 66%;
        min-height: 90vh;

        /* center horizontaly on the page */
        margin: auto;
        margin-top: 20px;
        margin-bottom: 20px;

        border-radius: 20px;

        padding: 20px;

        /* aligne the item verticaly */
        display: flex;
        flex-direction: column;
        align-items: center;

        background-color: white;

        border: 1px solid rgb(223, 223, 223);
    }

    .row {
        display: flex;
        flex-direction: row;

        justify-content: center;
    }

    .cell {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 100px;
        height: 66px;
        border: 1px solid black;
    }

    .newScore {
        padding: 0;
        width: 100px;
        height: 80px;
        border: 1px solid black;
        margin-top: 20px;
        text-align: center;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    input {
        text-align: center;
    }

    button {
        background-color: #405cf5;
        border-radius: 10px;
        border-width: 0;
        box-sizing: border-box;
        color: #fff;
        cursor: pointer;
        font-size: 100%;
        height: 44px;
        line-height: 1.15;
        margin: 12px 0 0;
        padding: 0 25px;
        text-align: center;
    }

    button:hover {
        background-color: #E01E79;
    }

    .total {
        font-weight: bold;
    }

    .bold {
        font-weight: bold;
    }
</style>
