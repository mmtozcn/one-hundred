<script>
    const Types = {
        NOT: 1,
        PLACEHOLDER: 2,
        SELECTED: 3
    }

    const Result = {
        ERROR: 0,
        SUCCESS: 1
    }

    const BOARD_SIZE = 10;

    export let result = null;
    let counter = 0;
    let active = [];
    let board = [];

    for (let x = 0; x < BOARD_SIZE; x++) {
        board.push([]);
        for (let y = 0; y < BOARD_SIZE; y++) {
            board[x].push(Types.NOT);
        }
    }

    const check = (x, y) => (e) => {
        if (counter === 0) {
            counter += 1;
            marker(x, y, e);
            boardCheck();
        } else if (board[x][y] === Types.PLACEHOLDER) {
            counter += 1;
            marker(x, y, e);
            boardCheck();
        }

        if (counter === BOARD_SIZE * BOARD_SIZE) {
            result = Result.SUCCESS;
        }
    }

    const marker = (x, y, e) => {
        e.target.innerText = counter;
        board[x][y] = Types.SELECTED;
        active = [x, y];
    }

    const boardCheck = () => {
        let check = false;

        const aX = active[0];
        const aY = active[1];

        for (let x = 0; x < BOARD_SIZE; x++) {
            for (let y = 0; y < BOARD_SIZE; y++) {
                if (board[x][y] !== Types.SELECTED) {
                    board[x][y] = Types.NOT;
                }

                if (aX + 3 < BOARD_SIZE && board[aX + 3][aY] === Types.NOT) {
                    board[aX + 3][aY] = Types.PLACEHOLDER;
                    check = true;
                }
                if (aX - 3 >= 0 && board[aX - 3][aY] === Types.NOT) {
                    board[aX - 3][aY] = Types.PLACEHOLDER;
                    check = true;
                }

                if (aY + 3 < BOARD_SIZE && board[aX][aY + 3] === Types.NOT) {
                    board[aX][aY + 3] = Types.PLACEHOLDER;
                    check = true;
                }
                if (aY - 3 >= 0 && board[aX][aY - 3] === Types.NOT) {
                    board[aX][aY - 3] = Types.PLACEHOLDER;
                    check = true;
                }

                if (aX - 2 >= 0 && aY - 2 >= 0 && board[aX - 2][aY - 2] === Types.NOT) {
                    board[aX - 2][aY - 2] = Types.PLACEHOLDER;
                    check = true;
                }
                if (aX + 2 < BOARD_SIZE && aY + 2 < BOARD_SIZE && board[aX + 2][aY + 2] === Types.NOT) {
                    board[aX + 2][aY + 2] = Types.PLACEHOLDER;
                    check = true;
                }

                if (aX + 2 < BOARD_SIZE && aY - 2 >= 0 && board[aX + 2][aY - 2] === Types.NOT) {
                    board[aX + 2][aY - 2] = Types.PLACEHOLDER;
                    check = true;
                }
                if (aX - 2 >= 0 && aY + 2 < BOARD_SIZE && board[aX - 2][aY + 2] === Types.NOT) {
                    board[aX - 2][aY + 2] = Types.PLACEHOLDER;
                    check = true;
                }
            }
        }
        if (!check) {
            result = Result.ERROR;
        }
    }

</script>

{#each board as boardX, x}
    {#each boardX as boardY, y}
        <button on:click={check(x, y)}
                class:active={board[x][y] === Types.SELECTED}
                class:placeholder={board[x][y] === Types.PLACEHOLDER}
        ></button>
    {/each}
{/each}
