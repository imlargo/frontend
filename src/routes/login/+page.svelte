<script lang="ts">
	import * as Card from '$lib/components/ui/card';
	import { Label } from '$lib/components/ui/label';
	import { Input } from '$lib/components/ui/input';
	import { Button } from '$lib/components/ui/button';
	import { onMount } from 'svelte';
	import { invalidateAll } from '$app/navigation';

	type LoginForm = {
		email: string;
		password: string;
	};

	let isLoading = $state(false);
	let form: HTMLFormElement;
	const formData: LoginForm = $state({
		email: '',
		password: ''
	});

	async function onsubmit() {
		isLoading = true;

		if (!formData.email || !formData.password) {
			alert('Por favor, rellena todos los campos');
		}

		const email = document.createElement('input');
		email.type = 'hidden';
		email.name = 'email';
		email.value = formData.email;
		form.appendChild(email);

		const password = document.createElement('input');
		password.type = 'hidden';
		password.name = 'password';
		password.value = formData.password;
		form.appendChild(password);

		form.submit();
	}

	/*
	 * Create form to request access token from Google's OAuth 2.0 server.
	 */
	function oauthSignIn() {
		// Google's OAuth 2.0 endpoint for requesting an access token
		const oauth2Endpoint = 'https://accounts.google.com/o/oauth2/v2/auth';

		// Create <form> element to submit parameters to OAuth 2.0 endpoint.
		const form = document.createElement('form');
		form.setAttribute('method', 'GET'); // Send as a GET request.
		form.setAttribute('action', oauth2Endpoint);

		// Parameters to pass to OAuth 2.0 endpoint.
		const params: Record<string, string> = {
			client_id: '927512412726-qbrkf1gcel5f2gnsk6tsegq8n1gjfni9.apps.googleusercontent.com',
			redirect_uri: `${window.location.origin}/authorize`,
			response_type: 'code',
			prompt: 'select_account',
			scope:
				'openid profile email https://www.googleapis.com/auth/user.birthday.read https://www.googleapis.com/auth/user.gender.read',
			include_granted_scopes: 'true'
		};

		// Add form parameters as hidden input values.
		for (const p in params) {
			const input = document.createElement('input');
			input.setAttribute('type', 'hidden');
			input.setAttribute('name', p);
			input.setAttribute('value', params[p]);
			form.appendChild(input);
		}

		// Add form to page and submit it to open the OAuth 2.0 endpoint.
		document.body.appendChild(form);
		form.submit();
	}

	onMount(async () => {
		await invalidateAll();
	});
</script>

<div class="flex min-h-screen items-center justify-center">
	<Card.Root class="w-full max-w-md">
		<Card.Header class="space-y-1">
			<Card.Title class="text-center text-3xl font-bold">Bienvenido de vuelta</Card.Title>
			<Card.Description class="text-center">
				Ingresa a tu cuenta para continuar aprendiendo
			</Card.Description>
		</Card.Header>
		<Card.Content>
			<form {onsubmit} class="space-y-4">
				<div class="space-y-2">
					<Label for="email">Usuario / Correo Electrónico</Label>
					<Input
						id="email"
						type="text"
						placeholder="m@example.com"
						bind:value={formData.email}
						required
					/>
				</div>
				<div class="space-y-2">
					<div class="flex items-center justify-between">
						<Label for="password">Contraseña</Label>
						<Button
							variant="link"
							href="/forgot-password"
							class="h-min text-sm text-blue-600 hover:underline"
						>
							¿Olvidaste tu contraseña?
						</Button>
					</div>
					<Input id="password" type="password" required bind:value={formData.password} />
				</div>
				<Button class="w-full font-semibold" type="submit" disabled={isLoading}>
					{#if isLoading}
						<span>Iniciando sesión...</span>
						<i class="bi bi-arrow-repeat animate-spin leading-none"></i>
					{:else}
						<span>Iniciar Sesión</span>
					{/if}
				</Button>
			</form>

			<div class="mt-4">
				<Button variant="outline" class="w-full" disabled={isLoading} onclick={oauthSignIn}>
					{#if isLoading}
						<i class="bi bi-arrow-repeat animate-spin leading-none"></i>
					{:else}
						<i class="bi bi-google leading-none"></i>
					{/if}
					<span>Google</span>
				</Button>
			</div>
		</Card.Content>
		<Card.Footer class="flex flex-col space-y-4">
			<div class="relative w-full">
				<div class="absolute inset-0 flex items-center">
					<span class="w-full border-t"></span>
				</div>
				<div class="relative flex justify-center text-xs uppercase">
					<span class="bg-background px-2 text-muted-foreground"> O </span>
				</div>
			</div>

			<div class="text-center text-sm text-gray-600">
				<span>¿No tienes una cuenta?</span>
				<Button
					variant="link"
					href="/register"
					class="px-1 font-semibold text-blue-600 hover:underline">Regístrate</Button
				>
			</div>
		</Card.Footer>
	</Card.Root>
</div>

<form method="post" bind:this={form}></form>
