<script lang="ts">
	import type { Usuario } from '$lib/types';

	type Props = {
		user: Usuario;
		page: string;
	};

	const { page, user }: Props = $props();

	import 'bootstrap-icons/font/bootstrap-icons.min.css';
	import * as DropdownMenu from '$lib/components/ui/dropdown-menu';
	import * as Avatar from '$lib/components/ui/avatar';
	import { Separator } from '$lib/components/ui/separator/index.js';
</script>

<header class="flex items-center justify-between border-b bg-white px-6 py-4">
	<div class="flex items-center space-x-4">
		<h1 class="text-2xl font-bold">
			{'Minerva' + (page ? ` - ${page}` : '')}
		</h1>
		<Separator orientation="vertical" />
		<a class="text-sm font-medium" href="/admin/cursos">Cursos</a>
	</div>

	<nav class="flex items-center space-x-4">
		<DropdownMenu.Root>
			<DropdownMenu.Trigger>
				<Avatar.Root class="size-11">
					<Avatar.Image src={user.avatar} alt={user.fullname} />
					<Avatar.Fallback
						>{user.fullname
							.split(' ')
							.map((name) => name[0])
							.join('')}</Avatar.Fallback
					>
				</Avatar.Root>
			</DropdownMenu.Trigger>
			<DropdownMenu.Content class="w-56" align="end">
				<DropdownMenu.Label class="font-normal">
					<div class="flex flex-col space-y-1">
						<p class="text-sm font-medium leading-none">{user.fullname}</p>
						<p class="text-xs leading-none text-muted-foreground">{user.email}</p>
					</div>
				</DropdownMenu.Label>
				<DropdownMenu.Separator />
				<DropdownMenu.Group>
					<DropdownMenu.Item>
						<i class="bi bi-person"></i>
						<span>Perfil</span>
					</DropdownMenu.Item>
					<DropdownMenu.Item>
						<i class="bi bi-gear"></i>
						<span>Configuracion</span>
					</DropdownMenu.Item>
				</DropdownMenu.Group>
				<DropdownMenu.Separator />
				<DropdownMenu.Item>
					<i class="bi bi-box-arrow-right"></i>
					<span>Cerrar sesion</span>
				</DropdownMenu.Item>
			</DropdownMenu.Content>
		</DropdownMenu.Root>
	</nav>
</header>

<style lang="scss">
	.nav-link {
		@apply rounded-md px-2 py-1 hover:bg-zinc-100;
	}
</style>
