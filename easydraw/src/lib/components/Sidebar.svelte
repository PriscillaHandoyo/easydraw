<script>
    let fileName = $state("Untitled");
    let searchBar = $state("");
</script>

<main>
    <input class="file-name"
           bind:value={fileName}
    />

    <input class="search-bar"
           bind:value={searchBar} placeholder="Search..."
    />

    <section class="basic-shapes">
        <h1>Basic Shapes</h1>
    </section>
</main>

<style>
    input {
        /* Removes the default border and background */
        appearance: none;
        -webkit-appearance: none;
        -moz-appearance: none;

        border: none;
        background: none;
        box-shadow: none;

        /* Inherits font settings from the parent */
        font: inherit;
        color: inherit;

        /* Removes default padding/margin */
        padding: 0;
        margin: 0;

        /* Ensures it doesn't have an outline when not focused */
        outline: none;
    }

    main {
        display: flex;
        flex-direction: column;
        gap: 1em;
        width: 300px;
        background-color: white;
        border: 3px solid black;
        border-radius: 10px;
        padding: 1em;
        position: fixed;
        top: 20px;
        left: 20px;
        z-index: 50;
        height: 90vh;
    }

    .file-name {
        font-weight: bold;
    }

    h1 {
        font-size: 1.2rem;
    }
</style>