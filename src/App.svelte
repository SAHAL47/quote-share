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

    function shareQuote(index) {
        const quote = quotes[index];
        const shareUrl = `https://quoteshare-mp70uoth3-sahal-kunnatteyils-projects.vercel.app/api/quote?text=${encodeURIComponent(quote.text)}&author=${encodeURIComponent(quote.author)}`;
        
        // Construct the Facebook share URL
        const facebookShareUrl = `https://www.facebook.com/dialog/share?app_id=1569555470261514&display=popup&href=${encodeURIComponent(shareUrl)}&redirect_uri=${encodeURIComponent(window.location.href)}`;

        // Open the Facebook share dialog in a new window
        window.open(facebookShareUrl, 'facebook-share-dialog', 'width=800,height=600');
    }

    onMount(() => {
        // Initialization for other parts of your app can be placed here
    });
</script>

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

/* Container styles */
.container {
    background: #fff;
    padding: 20px;
    border-radius: 8px;
    width: 90%; /* Responsive width */
    max-width: 400px; /* Maximum width */
    text-align: center;
    margin-bottom: 20px; /* Space between container and quotes */
}

/* Heading styles */
h1 {
    margin-bottom: 20px; /* Space below heading */
}

/* Form styles */
form {
    display: flex;
    flex-direction: column;
    align-items: center; /* Center form elements horizontally */
    margin-bottom: 20px; /* Space between form and quotes */
}

/* Input and button styles */
input, button {
    padding: 10px;
    margin: 5px 0;
    border: 1px solid #ccc;
    border-radius: 5px;
    width: 100%;
    max-width: 300px; /* Max width for inputs and buttons */
}

input:focus, button:focus {
    outline: none;
    border-color: #007bff;
}

/* Quote container styles */
.quote-container {
    width: 90%; /* Responsive width */
    max-width: 400px; /* Maximum width */
    display: flex;
    flex-direction: column;
    align-items: center; /* Center quotes horizontally */
    overflow-y: auto; /* Allow scrolling if there are many quotes */
}

/* Individual quote styles */
.quote-template {
    position: relative;
    padding: 20px;
    background: url('/pexel.jpg') no-repeat center center;
    background-size: cover;
    border-radius: 8px;
    color: white;
    text-align: center;
    margin: 10px 0;
    width: 100%; /* Full width of the container */
    box-sizing: border-box; /* Include padding and border in the element's total width and height */
}

/* Text inside quote */
.quote-template span {
    display: block;
    font-size: 1.2em; /* Adjusted font size for responsiveness */
    font-style: italic;
    margin-bottom: 10px;
}

.quote-template .author {
    font-size: 1em; /* Adjusted font size for responsiveness */
    font-weight: bold;
}

/* Controls styles */
.controls {
    margin-top: 10px;
}

.controls button {
    padding: 8px 16px; /* Adjusted padding */
    margin: 5px;
    background: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 0.9em; /* Adjusted font size */
}

.controls button:hover {
    background: #0056b3;
}

/* Media queries for responsiveness */
@media (max-width: 600px) {
    .container {
        width: 95%;
        max-width: none; /* Remove maximum width for small screens */
    }

    .quote-container {
        width: 95%;
        max-width: none; /* Remove maximum width for small screens */
    }

    .quote-template {
        font-size: 1em; /* Adjust font size for smaller screens */
    }

    .quote-template span {
        font-size: 1em; /* Adjust font size for smaller screens */
    }

    .quote-template .author {
        font-size: 0.9em; /* Adjust font size for smaller screens */
    }
}
</style>

<div class="container">
    <h1>Quote Sharing App</h1>

    <!-- Form to add new quotes -->
    <form on:submit={addQuote}>
        <input type="text" bind:value={newQuote} placeholder="Enter your quote" required>
        <input type="text" bind:value={personName} placeholder="Your name" required>
        <button type="submit">Add Quote</button>
    </form>
</div>

<!-- Container to display quotes -->
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