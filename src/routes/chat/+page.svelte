<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { goto } from '$app/navigation';
	import { currentUser, pb } from '../../lib/pocketbase';
	import '@fortawesome/fontawesome-free/css/all.css';

	let newMessage: string;
	let messages: any[] = [];
	let unsubscribe: () => void;

	onMount(async () => {
		if (!$currentUser) {
			goto('/login');
		}

		// Get initial messages
		const resultList = await pb.collection('messages').getList(1, 50, {
			sort: 'created',
			expand: 'user'
		});
		messages = resultList.items;

		// Subscribe to realtime messages
		unsubscribe = await pb.collection('messages').subscribe('*', async ({ action, record }) => {
			if (action === 'create') {
				// Fetch associated user
				const user = await pb.collection('users').getOne(record.user);
				record.expand = { user };
				messages = [...messages, record];
			}
			if (action === 'delete') {
				messages = messages.filter((m) => m.id !== record.id);
			}
		});
	});

	// Unsubscribe from realtime messages
	onDestroy(() => {
		unsubscribe?.();
	});

	async function sendMessage() {
		const data = {
			text: newMessage,
			user: $currentUser?.id
		};
		const createdMessage = await pb.collection('messages').create(data);
		newMessage = '';
	}

	async function deleteMessage(id: string) {
		await pb.collection('messages').delete(id);
	}
</script>

{#if $currentUser}
	<div class="chat-container">
		<div class="chat-header">
			<h2>Messages</h2>
			<button class="toggle-theme-button">
				<i class="fas fa-sun" />
				<i class="fas fa-moon" />
			</button>
		</div>

		<div class="messages">
			{#each messages as message (message.id)}
				<div class="message">
					<img
						class="avatar"
						src={`https://avatars.dicebear.com/api/identicon/${message.expand?.user?.username}.svg`}
						alt="avatar"
						width="40px"
					/>
					<div class="message-content">
						<div class="message-header">
							<p class="username">@{message.expand?.user?.username}</p>
							<p class="timestamp">
								{new Date(message.created).toLocaleTimeString()}
							</p>
						</div>
						<p class="text">{message.text}</p>
					</div>
					{#if message.expand?.user?.id === $currentUser.id}
						<button
							class="delete-message-button"
							title="Delete"
							on:click={() => deleteMessage(message.id)}
						>
							<i class="fas fa-trash" />
						</button>
					{/if}
				</div>
			{/each}
		</div>

		<form class="message-input-form" on:submit|preventDefault={sendMessage}>
			<input placeholder="Type your message here..." type="text" bind:value={newMessage} />
			<button type="submit">
				<i class="fas fa-paper-plane" />
			</button>
		</form>
	</div>
{:else}
	<p>You need to log in to access this page.</p>
{/if}

<style>
	/* Chat Container */
	.chat-container {
		max-width: 500px;
		margin: 0 auto;
		padding: 16px;
		background-color: #1d1d1f;
		color: #fff;
		border-radius: 8px;
	}

	/* Chat Header */
	.chat-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 16px;
	}

	.chat-header h2 {
		margin: 0;
		font-size: 24px;
		font-weight: 600;
	}

	.toggle-theme-button {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 40px;
		height: 40px;
		background-color: #333;
		color: #fff;
		border-radius: 50%;
		cursor: pointer;
		border: none;
		outline: none;
	}

	.toggle-theme-button i {
		font-size: 16px;
	}

	/* Messages */
	.messages {
		display: flex;
		flex-direction: column;
		gap: 8px;
		overflow-y: auto;
		max-height: 400px;
	}

	.message {
		display: flex;
		align-items: flex-start;
		gap: 8px;
		border-radius: 8px;
		padding: 8px;
		background-color: #2b2b2f;
	}

	.message .avatar {
		border-radius: 50%;
	}

	.message-content {
		display: flex;
		flex-direction: column;
		gap: 4px;
		width: 100%;
	}

	.message-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.message-header .username {
		margin: 0;
		font-size: 16px;
		font-weight: 600;
	}

	.message-header .timestamp {
		margin: 0;
		font-size: 12px;
		color: #9a9a9d;
	}

	.message .text {
		margin: 0;
		font-size: 14px;
	}

	.delete-message-button {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 30px;
		height: 30px;
		background-color: #333;
		color: #fff;
		border-radius: 50%;
		cursor: pointer;
		border: none;
		outline: none;
	}

	.delete-message-button i {
		font-size: 14px;
	}

	/* Message Input Form */
	.message-input-form {
		display: flex;
		align-items: center;
		gap: 8px;
		margin-top: 16px;
	}

	.message-input-form input {
		flex: 1;
		padding: 8px;
		background-color: #2b2b2f;
		border: none;
		color: #fff;
		border-radius: 8px;
		outline: none;
	}

	.message-input-form button {
		width: 40px;
		height: 40px;
		background-color: #333;
		color: #fff;
		border-radius: 50%;
		cursor: pointer;
		border: none;
		outline: none;
		display: flex; /* center the icon horizontally */
		justify-content: center; /* center the icon vertically */
	}

	.message-input-form button i {
		font-size: 16px;
	}
</style>
