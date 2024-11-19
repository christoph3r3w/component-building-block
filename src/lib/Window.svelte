
<script lang=ts>

	interface Props {
		title: string;
		context: string;
		role: string;
		text:string;
		class: string;
		color:string;
		children?: import('svelte').Snippet<[]>; 
	}

	let {title,context,role,text, children, class: CLASS,color, ...props} : Props = $props(); 

</script>

{#snippet A()}
	{#if title}
		<h1>{title}</h1>
	{/if}
	{#if context}
		<p>{context}</p>
	{/if}
	{#if children}
	<section>
		{@render children()}
	</section>
	{/if}
{/snippet}

{#snippet B()}

	{#if title}
	<div class="header">
		<h1>{title}</h1>
		<p>{context}</p>
	</div>
	{/if}

	{#if children}
	<section>
		{@render children()}
	</section>
	{/if}
{/snippet}

{#if role == 'window'}
	<!-- window container -->
	<main class="contain {CLASS}" style="background-color:{color};" {...props} >
		{@render B()}
	</main>
{:else if role == 'child'}
	<!-- any container inside the main container -->
	<div class="contain child-container {CLASS}" style="background-color:{color};" {...props}>
		{@render A()}
	</div>
{:else if role == 'buttonBox'}
	<!-- for containers with multiple buttons -->
	<div class="contain buttonBox {CLASS}" style="background-color:{color};" {...props}>
		{@render A()}
	</div>
{:else if role == 'header'}
	<!-- for the main header content of a section  -->
	<div class="contain header {CLASS}" {...props}>
		{@render B()}
	</div>
{/if}


<style>

	*{
	--wc-radius:clamp(0.5rem, 2vw, 1.8pc);
	--wc-bg:rgb(108, 108, 255);
	--cc-bg:rgb(250, 255, 114);
	/* --cc-radius:; */
	}

	.contain{
		container-type:inline-size;
		position: relative;
		padding: 2%;
	}

	main{
		display: grid;
		grid-template-columns:[full-start] minmax(1px, .1fr) [context-start] repeat(2,1fr) [context-end] minmax(1px, .1fr) [full-end];
		grid-template-rows: 5rem auto;
		width: 100%;
		height: 100dvh;

		container-name:window-container;

		background-color:var(--wc-bg,white) ;
		color: var(--wc-text,black);
		border-radius: var(--wc-radius,inherit);

		& > :is(:nth-child(n)){
		grid-column: context;
		}
	}

	main >:nth-child(n){
		grid-column: context;
		border-radius: var(--cc-radius,inherit);
	}

	main >:nth-child(1):is(.header),.header{
		grid-column: context;
		outline: solid;
		height: fit-content;
		text-align: center;

		&:is(:focus-visible,:focus-within){
			outline: dotted rgba(255, 0, 0, 0.112);
		}
	}

	main section{
		height: 100%;
		border-radius: var(--cc-radius,inherit);

	}

	main .buttonBox{
		--cc-radius:1pc;
		--w:45;
		--p: calc(calc(100 - var(--w))/2);
		display: flex;
		min-width: fit-content;
		/* width: min-content; */
		/* height: fit-content; */
		/* justify-content: center ;  */
		align-self: center; 
		padding-block: 3cqw;
		
		/* position: absolute; */
		inset-inline-start: calc(var(--p) * 1%);
		bottom: 0;
		width: calc(var(--w) * 1% );
		height: auto;
		border-radius: var(--cc-radius,inherit);

	}

	.child-container{
		display: flex;
		flex-direction: column;

		container-name:child-contianer;

		background-color:var(--cc-bg,white) ;
		color: var(--cc-text,black);
		border-radius: var(--cc-radius,inherit);
		outline: 1px solid;
	}

