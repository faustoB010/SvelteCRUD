<script>
  import Breadcrumb from "sveltestrap/src/Breadcrumb.svelte";
  import BreadcrumbItem from "sveltestrap/src/BreadcrumbItem.svelte";
  import Card from "sveltestrap/src/Card.svelte";
  import CardBody from "sveltestrap/src/CardBody.svelte";
  import CardHeader from "sveltestrap/src/CardHeader.svelte";
  import CardText from "sveltestrap/src/CardText.svelte";
  import CardSubtitle from "sveltestrap/src/CardSubtitle.svelte";
  import Row from "sveltestrap/src/Row.svelte";

  import DashboardCard from "../components/DashboardCard.svelte";
  import CustomCard from "../components/CustomCard.svelte";
  import Table from "../components/Table.svelte";
  import AreaChart from "../components/Charts/AreaChart.svelte";
  import BarChart from "../components/Charts/BarChart.svelte";

  let title = "Préstamo Películas Svelte";
</script>

<svelte:head>
  <title>{title}</title>
</svelte:head>
<h1 class="mt-4" align="center">Sistema Prestamista de Películas</h1>

<Row>
  <div class="col-xl-12">
    <CustomCard  cardTitle="¡Bienvenido!" >
      <p align="center">A continuación se mostrará las distintas tablas de la base de datos, divididas en: Idioma, Películas, Género y Préstamos.</p> <p align="center">¡Navega libremente por el sistema!
      
    </CustomCard>
  </div>
</Row>

