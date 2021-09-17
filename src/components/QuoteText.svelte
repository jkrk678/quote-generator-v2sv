<script>
    import Button from './Button.svelte';
    import Loader from './Loader.svelte';

    let apiQuotes = [];
    let author = '';
    let quote = '';

    const newQuote = () => {
        const newQuote = apiQuotes[Math.floor(Math.random() * apiQuotes.length)];
        author = newQuote.author === null ? 'Unknown' : newQuote.author;
        quote = newQuote.text;
    }
    async function getQuotes() {
        const apiUrl = 'https://type.fit/api/quotes';
        try {
            const response = await fetch(apiUrl);
            apiQuotes = await response.json();
            newQuote();
        } catch (error) {
            alert(`There is an error. The error is: ${error}`);
        }
    }

    const tweetQuote = () => {
        const twitterUrl = `https://twitter.com/intent/tweet?text=${quote} - ${author}`;
        window.open(twitterUrl, '_blank');
    };
</script>

{#await getQuotes()}
<Loader />
{:then apiQuotes}
<div>
    <i class="fas fa-quote-left"></i>
    <span class={quote.length > 120 ? 'long-text' : 'text'}>{quote}</span>

    <div class="author">
        <span>{author}</span>
    </div>

    <div class="buttons">
        <button class="twitter-button" on:click={tweetQuote} aria-label="Tweet this quote">
            <i class="fab fa-twitter"></i>
        </button>
        <Button on:click={newQuote} />
    </div>
</div>
{/await}

<style>
    .buttons {
        display: flex;
        justify-content: space-between;
    }

    .text {
        font-size: 2.75rem;
    }

    .long-text {
        font-size: 2rem;
    }

    .fa-quote-left {
        font-size: 4rem;
        margin-right: 8px;
    }

    .author {
        margin: 15px 0;
        font-size: 2rem;
        font-style: italic;
    }

    .twitter-button:hover {
        color: #38a1f3;
    }

    @media screen and (max-width: 1000px) {
        .text {
            font-size: 2.5rem;
        }
    }
</style>