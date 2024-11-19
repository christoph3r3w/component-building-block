
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
