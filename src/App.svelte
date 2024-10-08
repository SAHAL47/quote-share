<script>
    import { onMount } from 'svelte';

    let quotes = JSON.parse(localStorage.getItem('quotes')) || [];
    let newQuote = '';
    let personName = '';

    function addQuote(event) {
        event.preventDefault();
        if (newQuote && personName) {
            quotes = [...quotes, { text: newQuote, author: personName }];
            localStorage.setItem('quotes', JSON.stringify(quotes));
            newQuote = '';
            personName = '';
        }
    }

    function removeQuote(index) {
        quotes = quotes.filter((_, i) => i !== index);
        localStorage.setItem('quotes', JSON.stringify(quotes));
    }

    async function shareQuote(index) {
        const quote = quotes[index];
        const shareData = {
            title: 'Quote Sharing App',
            text: `"${quote.text}" - ${quote.author}`,
            url: 'https://quoteshare-beta.vercel.app'
        };

        try {
            await navigator.share(shareData);
            alert('Quote shared successfully');
        } catch (err) {
            alert('Error while sharing quote: ' + err.message);
        }
    }
</script>

<svelte:head>
    <title>Quote Sharing App</title>
    <meta property="og:title" content="Quote Sharing App" />
    <meta property="og:description" content="Share your favorite quotes with friends." />
    <meta property="og:image" content="https://quoteshare-mp70uoth3-sahal-kunnatteyils-projects.vercel.app/pexel.jpg" />
    <meta property="og:url" content="https://quoteshare-mp70uoth3-sahal-kunnatteyils-projects.vercel.app" />
    <meta property="og:type" content="website" />

    <!-- JSON-LD Structured Data -->
    {#if quotes.length > 0}
        <script type="application/ld+json">
            {JSON.stringify({
                "@context": "https://schema.org",
                "@type": "CreativeWork",
                "name": "Quote Sharing App",
                "description": "Share your favorite quotes with friends.",
                "mainEntity": quotes.map(quote => ({
                    "@type": "Quotation",
                    "text": quote.text,
                    "author": {
                        "@type": "Person",
                        "name": quote.author
                    }
                }))
            })}
        </script>
    {/if}
</svelte:head>

<style>
    :global(body) {
        font-family: Arial, sans-serif;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100vh;
        background-color: #f4f4f4;
        margin: 0;
    }

    .container {
        background: #fff;
        padding: 20px;
        border-radius: 8px;
        width: 90%;
        max-width: 400px;
        text-align: center;
        margin-bottom: 20px;
    }

    h1 {
        margin-bottom: 20px;
    }

    form {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-bottom: 20px;
    }

    input, button {
        padding: 10px;
        margin: 5px 0;
        border: 1px solid #ccc;
        border-radius: 5px;
        width: 100%;
        max-width: 300px;
    }

    input:focus, button:focus {
        outline: none;
        border-color: #007bff;
    }

    .quote-container {
        width: 90%;
        max-width: 400px;
        display: flex;
        flex-direction: column;
        align-items: center;
        overflow-y: auto;
    }

    .quote-template {
        position: relative;
        padding: 20px;
        background: url('/pexel.jpg') no-repeat center center;
        background-size: cover;
        border-radius: 8px;
        color: white;
        text-align: center;
        margin: 10px 0;
        width: 100%;
        box-sizing: border-box;
    }

    .quote-template span {
        display: block;
        font-size: 1.2em;
        font-style: italic;
        margin-bottom: 10px;
    }

    .quote-template .author {
        font-size: 1em;
        font-weight: bold;
    }

    .controls {
        margin-top: 10px;
    }

    .controls button {
        padding: 8px 16px;
        margin: 5px;
        background: #007bff;
        color: #fff;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-size: 0.9em;
    }

    .controls button:hover {
        background: #0056b3;
    }

    @media (max-width: 600px) {
        .container {
            width: 95%;
            max-width: none;
        }

        .quote-container {
            width: 95%;
            max-width: none;
        }

        .quote-template {
            font-size: 1em;
        }

        .quote-template span {
            font-size: 1em;
        }

        .quote-template .author {
            font-size: 0.9em;
        }
    }
</style>

<div class="container">
    <h1>Quote Sharing App</h1>

    <form on:submit={addQuote}>
        <input type="text" bind:value={newQuote} placeholder="Enter your quote" required>
        <input type="text" bind:value={personName} placeholder="Your name" required>
        <button type="submit">Add Quote</button>
    </form>
</div>

<div id="quote-container" class="quote-container">
    {#each quotes as quote, index}
        <div class="quote-template">
            <span>"{quote.text}"</span>
            <div class="author">- {quote.author}</div>
            <div class="controls">
                <button on:click={() => shareQuote(index)}>Share</button>
                <button on:click={() => removeQuote(index)}>Remove</button>
            </div>
        </div>
    {/each}
</div>


