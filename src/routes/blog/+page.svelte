<script lang="ts">
	import type { PageServerData } from './$types';
	import * as Card from '$lib/components/ui/card';
	import * as Tooltip from '$lib/components/ui/tooltip';
	import dayjs from 'dayjs';
	import relativeTime from 'dayjs/plugin/relativeTime';

	export let data: PageServerData;

	dayjs.extend(relativeTime);

	const now = dayjs();
</script>

<header class="relative flex min-h-48 flex-col items-center justify-center text-center">
	<h1 class="mb-4 text-5xl font-bold leading-tight">Blog</h1>
</header>

<div class="relative mx-16 flex flex-wrap items-center justify-center gap-4 sm:mx-32 lg:mx-64">
	{#each data.posts as post}
		<article class="w-full rounded-lg border bg-card text-card-foreground shadow-sm">
			<div class="flex flex-col space-y-1.5 p-6">
				<a class="hover:underline" href="/blog/{post.slug}">
					<h3 class="text-lg font-semibold leading-none tracking-tight">{post.title}</h3>
				</a>
				<p class="text-sm text-muted-foreground">
					<Tooltip.Root>
						{@const date = dayjs(post.date)}

						<Tooltip.Trigger>
							{now.diff(date, 'days', true) < 7 // Posted in the last week
								? date.from(now) // Show relative time
								: date.format('DD/MMM/YYYY [at] HH:mm')}
						</Tooltip.Trigger>
						<Tooltip.Content>
							{date.format('dddd, MMMM DD, YYYY [at] HH:mm')}
						</Tooltip.Content>
					</Tooltip.Root>
				</p>
			</div>
			<div class="p-6 pt-0">{post.description}</div>
			<div class="flex items-center p-6 pt-0">
				<a class="hover:underline" href="/blog/{post.slug}">Read more</a>
			</div>
		</article>
	{/each}
</div>
