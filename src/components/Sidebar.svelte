<script>
  import Collapse from "sveltestrap/src/Collapse.svelte";
  import Nav from "sveltestrap/src/Nav.svelte";

  import SidebarItem from "./SidebarItem.svelte";

  export let segment;
  export let theme;

  $: sidenav_theme = `sb-sidenav-${theme}`;

  let isLayoutOpen = false;
  let isPageOpen = false;
  let isAuthenticationOpen = false;
  let isErrorOpen = false;
  let activeLink = "Dashboard";
  let footerName = "Administrador";
  let footerText = "Ingreso como:";

  const updateActiveLink = linkName => (activeLink = linkName);

  const toggleLayout = () => {
    isLayoutOpen = !isLayoutOpen;
    if (isPageOpen === true) isPageOpen = false;
  };

  const togglePages = () => {
    isPageOpen = !isPageOpen;
    if (isLayoutOpen === true) isLayoutOpen = false;
    if (isPageOpen === false) {
      isAuthenticationOpen = false;
      isErrorOpen = false;
    }
  };

  const toggleAuthentication = () => {
    isAuthenticationOpen = !isAuthenticationOpen;
    if (isErrorOpen === true) isErrorOpen = false;
  };

  const toggleError = () => {
    isErrorOpen = !isErrorOpen;
    if (isAuthenticationOpen === true) isAuthenticationOpen = false;
  };
</script>

<div id="layoutSidenav_nav" class="sb-nav-fixed">
  <Nav
    class="sb-sidenav {sidenav_theme} accordion sb-nav-fixed"
    id="sidenavAccordion">
    <div class="sb-sidenav-menu">
      <Nav>
        <div class="sb-sidenav-menu-heading">Home</div>
        <SidebarItem
          on:press={() => {
            theme = 'dark';
          }}
          text="Inicio"
          class={segment === '.' || segment === undefined ? 'active' : ''}
          leftIcon
          href=".">
          <i class="fas fa-tachometer-alt" slot="leftIcon" />
        </SidebarItem>
        <Collapse isOpen={isPageOpen}>
          <Nav
            class="sb-sidenav-menu-nested accordion"
            id="sidenavAccordionPages">
            <SidebarItem
              on:press={toggleAuthentication}
              class={!isAuthenticationOpen ? 'collapsed' : ''}
              text="Authentication"
              rightIcon>
              <i class="fas fa-angle-down" slot="rightIcon" />
            </SidebarItem>
            <Collapse isOpen={isAuthenticationOpen}>
              <Nav class="sb-sidenav-menu-nested">
                <SidebarItem href="pages/authentication/login" text="Login" />
                <SidebarItem
                  href="pages/authentication/register"
                  text="Register" />
                <SidebarItem
                  href="pages/authentication/forget_password"
                  text="Forgot Password" />
              </Nav>
            </Collapse>
            <SidebarItem
              on:press={toggleError}
              class={!isErrorOpen ? 'collapsed' : ''}
              text="Error"
              rightIcon>
              <i class="fas fa-angle-down" slot="rightIcon" />
            </SidebarItem>
            <Collapse isOpen={isErrorOpen}>
              <Nav class="sb-sidenav-menu-nested">
                <SidebarItem href="pages/error/error_401" text="401 Page" />
                <SidebarItem href="pages/error/error_404" text="404 Page" />
                <SidebarItem href="pages/error/error_500" text="500 Page" />
              </Nav>
            </Collapse>
          </Nav>
        </Collapse>
        <div class="sb-sidenav-menu-heading">Tablas</div>
        <SidebarItem
          class={segment === 'idioma' ? 'active' : ''}
          on:press={() => {
            theme = 'dark';
          }}
          href="idioma"
          text="Idiomas"
          leftIcon>
          <i class="fas fa-table" slot="leftIcon" />
        </SidebarItem>
        <SidebarItem
          class={segment === 'pelicula' ? 'active' : ''}
          on:press={() => {
            theme = 'dark';
          }}
          href="pelicula"
          text="Películas"
          leftIcon>
          <i class="fas fa-table" slot="leftIcon" />
        </SidebarItem>
        <SidebarItem
          class={segment === 'genero' ? 'active' : ''}
          on:press={() => {
            theme = 'dark';
          }}
          href="genero"
          text="Género"
          leftIcon>
          <i class="fas fa-table" slot="leftIcon" />
        </SidebarItem>
        <SidebarItem
          class={segment === 'prestamo' ? 'active' : ''}
          on:press={() => {
            theme = 'dark';
          }}
          href="prestamo"
          text="Préstamos"
          leftIcon>
          <i class="fas fa-table" slot="leftIcon" />
        </SidebarItem>
      </Nav>
    </div>
    <div class="sb-sidenav-footer">
      <div class="small">{footerText}</div>
      {footerName}
    </div>
  </Nav>
</div>
