
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


