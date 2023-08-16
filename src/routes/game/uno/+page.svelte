<script lang="ts">
    import { goto } from "$app/navigation";

    let nbPlayer = 2;
    let nbPointToStop = -1;
    let playersName: string[] = [];

    const redirectToScore = () => {

        // check if the number of player is correct
        if (nbPlayer < 2) {
            alert("You need at least 2 players");
            return;
        }

        if (nbPlayer > 10) {
            alert("You can't have more than 10 players");
            return;
        }

        // check if the name of the players are correct
        if (playersName.length !== nbPlayer) {
            alert("You need to enter a name for each player");
            return;
        }

        // check name lenght
        for (let i = 0; i < playersName.length; i++) {
            const name = playersName[i];
            if (name.length <= 2) {
                alert("The name of the player need to be at least 2 characters");
                return;
            }
            if (name.length > 10) {
                alert("The name of the player need to be less than 10 characters");
                return;
            }
        }

        localStorage.setItem("nbPlayers", nbPlayer.toString());
        localStorage.setItem("nbPointToStop", nbPointToStop.toString());
        localStorage.setItem("playersName", JSON.stringify(playersName));

        goto("/game/uno/score");
    };
</script>

<main>
    <h1>Uno</h1>

    <form on:submit|preventDefault={e => {
        e.preventDefault();
        redirectToScore();
    }}>
        <div class="inputs">
            <label for="nbPlayer">Number of player</label>
            <input type="number" id="nbPlayer" bind:value={nbPlayer} min="2" max="10" />
        </div>

        <div class="inputs">
            <label for="pointsMax">Point max to stop (-1 = infinite)</label>
            <input type="number" id="pointsMax" bind:value={nbPointToStop} min="-1" />
        </div>

        <!-- choose the name for each player -->
        {#each Array(nbPlayer) as _, i}
            <div class="inputs">
                <label for="player{i}">Player {i + 1} :</label>
                <input type="text" bind:value={playersName[i]} />
            </div>
        {/each}

        <button type="submit">Play</button>
    </form>
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

    form {
        display: flex;
        flex-direction: column;
        align-items: center;
        align-items: flex-start;
    }

    .inputs {
        display: flex;
        flex-direction: row;
        align-items: center;

        gap: 10px;

        margin-bottom: 10px;
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
</style>
