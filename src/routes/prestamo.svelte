<script>
    import Card from "sveltestrap/src/Card.svelte";
    import CardBody from "sveltestrap/src/CardBody.svelte";
    import CardHeader from "sveltestrap/src/CardHeader.svelte";
    import Form from "sveltestrap/src/Form.svelte";
    import FormGroup from "sveltestrap/src/FormGroup.svelte";
    import Label from "sveltestrap/src/Label.svelte";
    import Input from "sveltestrap/src/Input.svelte";

    let prestamos = [];
    let lista_peliculas=[];
    let peliculaSeleccionada;
    let activado = true;
    let datosPrestamo = {
        prestamo_id: null,
        prestada_el: "",
        devuelta_el: "",
        pelicula_id: "",
        socio: "",
    };


    function cargarPeliculas() {
     fetch("https://localhost:44315/api/Pelicula/")
            .then((respuesta) => respuesta.json())
            .then((datosPelicula) => {
                lista_peliculas = datosPelicula;
                
            })
            .catch(console.log);

         
    }
    let mostrarPrestamo = () => {
        fetch("https://localhost:44315/api/Prestamos/")
            .then((respuesta) => respuesta.json())
            .then((datosPrestamo) => {
                prestamos = datosPrestamo;
                datosPrestamo = {
                    prestamo_id: null,
                    prestada_el: "",
                    devuelta_el: "",
                    pelicula_id: "",
                    socio: "",
                };
                console.log(prestamos);
            })
            .catch(console.log);
            
    };

    let agregarPrestamo = () => {


        var comboPeliculas = document.getElementById("comboPeliculas");
       


        datosPrestamo.pelicula_id = retornarIDpel(
            comboPeliculas.options[comboPeliculas.selectedIndex].text
        );

      
        const nuevoPrestamo = {
            prestamo_id: null,
            prestada_el: datosPrestamo.prestada_el,
            devuelta_el: datosPrestamo.devuelta_el,
            pelicula_id: datosPrestamo.pelicula_id,
            socio: datosPrestamo.socio,

        };
        fetch("https://localhost:44315/api/Prestamos/", {
            method: "POST",
            headers: { "Content-type": "application/json; charset=UTF-8" },
            body: JSON.stringify(nuevoPrestamo),
        })
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                console.log(nuevoPrestamo);
                console.log(datosRespuesta);
                mostrarPrestamo();
            },alert("El préstamo ha sido creado con éxito"),
                window.location.reload())
            .catch(console.log);

    };

    let borrarPrestamo = (id) => {
        fetch("https://localhost:44315/api/Prestamos/" + id, {
            method: "DELETE",
        })
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                mostrarPrestamo();
            },alert("El préstamo ha sido eliminado con éxito"),
                window.location.reload())
            .catch(console.log);
            window.location.reload();
            
    };

    let editarPrestamo = (prestamo) => {

        datosPrestamo = prestamo;
        var comboPeliculas = document.getElementById("comboPeliculas");
        
        
        
        const gen = retornarPelDesc(datosPrestamo.pelicula_id);
        gen.then(
            (json) =>
                (comboPeliculas.options[comboPeliculas.selectedIndex].text =
                    json.titulo)
        );

       
    };
    let actualizarPrestamo = () => {


       
        var comboPeliculas = document.getElementById("comboPeliculas");

        datosPrestamo.pelicula_id = retornarIDpel(
            comboPeliculas.options[comboPeliculas.selectedIndex].text
        );

      
        fetch(
            "https://localhost:44315/api/Prestamos/" + datosPrestamo.prestamo_id,
            {
                method: "PUT",
                headers: { "Content-type": "application/json; charset=UTF-8" },
                body: JSON.stringify(datosPrestamo),
            },
        )
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                console.log(nuevoPrestamo);
                console.log(datosRespuesta);
                mostrarPrestamo();
            },alert("El préstamo ha sido actualizado con éxito"),
                window.location.reload())
            .catch(console.log);
    };

    async function retornarPelDesc(id) {
        const response = await fetch(
            "https://localhost:44315/api/Pelicula/" + id
        );

        return response.json();
    }

    function retornarIDpel(pel_id) {
        let respuesta = 0;

        for (let i = 0; i < lista_peliculas.length; i++) {
            let a = lista_peliculas[i];

            if (a.titulo === pel_id) {
                respuesta = a.pelicula_id;
            }
        }
        return respuesta;
    }
    cargarPeliculas();
    mostrarPrestamo();
</script>

<h1 class="mt-4">Préstamos</h1>

<Card class="mb-4">
    <CardHeader>
        <i class="fas fa-table" />
        Préstamos
    </CardHeader>
    <CardBody>
        <div class="container text-center">
            <div class="table-responsive">
                <table class="table table-striped">
                    <thead>
                        <tr>
                            <th>Préstamo ID</th>
                            <th>Fecha de préstamo</th>
                            <th>Fecha de devolución</th>
                            <th>Película</th>
                            <th>Socio</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody>
                        {#each prestamos as prestamo}
                            <tr>
                                <td>{prestamo.prestamo_id}</td>
                                <td>{prestamo.prestada_el}</td>
                                <td>{prestamo.devuelta_el}</td>
                                {#await retornarPelDesc(prestamo.pelicula_id)}
                                    <p>cargando....</p>
                                {:then data}
                                    <td>{data.titulo}</td>
                                {:catch error}
                                    <p>Algo no ha ido bien!XXXX{error}</p>
                                {/await}
                                <td>{prestamo.socio}</td>
                                <td>
                                    <FormGroup>
                                        <button
                                            on:click|preventDefault={editarPrestamo(
                                                prestamo
                                            )}
                                            type="submit"
                                            class="btn btn-secondary"
                                        >
                                            Editar
                                            
                                        </button>
                                    </FormGroup>
                                    <FormGroup>
                                        <button
                                            on:click|preventDefault={borrarPrestamo}
                                            type="submit"
                                            class="btn btn-dark"
                                            on:click={borrarPrestamo(
                                                prestamo.prestamo_id
                                            )}
                                        >
                                            Borrar
                                        </button>
                                    </FormGroup>
                                </td>
                            </tr>
                        {/each}
                    </tbody>
                </table>
            </div>
        </div></CardBody
    >
</Card>
<Card class="mb-4" >
    <CardHeader>
        <i class="fas fa-edit" />
        Crear/Editar
    </CardHeader>
    <CardBody>
        <div align="center">
            <Form>
                <FormGroup>
                    <Label for="name" class="small mb-1">Préstamo ID</Label>
                    <Input
                        readonly
                        type="number"
                        bind:value={datosPrestamo.prestamo_id}
                        class="label col-sm-3 col-form-label"
                        name="prestamo_id"
                        placeholder="Préstamo ID"
                    />
                </FormGroup>
                <FormGroup>
                    <Label for="name" class="small mb-1">Fecha de préstamo</Label>
                    <Input
                        type="date"
                        bind:value={datosPrestamo.prestada_el}
                        class="label col-sm-3 col-form-label"
                        name="prestada_el"
                        placeholder="Fecha Préstamo"
                    />
                </FormGroup>
                <FormGroup>
                    <Label for="name" class="small mb-1">Fecha de Devolución</Label>
                    <Input
                        type="date"
                        bind:value={datosPrestamo.devuelta_el}
                        class="label col-sm-3 col-form-label"
                        name="devuelta_el"
                        placeholder="Fecha devolución"
                    />
                </FormGroup>
                <Label for="name" class="small mb-1">Peliculas</Label> <br>
                <select class="label col-sm-3 col-form-label" value={peliculaSeleccionada} id="comboPeliculas" on:change={cargarPeliculas}>
                    {#each lista_peliculas as pel}
                        <option value={pel.pelicula_id}>
                            {pel.titulo}
                        </option>
                    {/each}
                </select>
                <br/>
                <br>
                <FormGroup>
                    <Label for="name" class="small mb-1">Socio</Label>
                    <Input
                        type="text"
                        bind:value={datosPrestamo.socio}
                        class="label col-sm-3 col-form-label"
                        name="socio"
                        placeholder="Socio"
                    />
                </FormGroup>
                
                
                <FormGroup>
                    <button
                        type="button"
                        class="btn btn-secondary"
                        on:click|preventDefault={agregarPrestamo}
                        >Agregar</button
                    >
                    <button
                        type="button"
                        class="btn btn-dark"
                        on:click|preventDefault={actualizarPrestamo}
                        >Actualizar</button
                    >
                </FormGroup>
            </Form>
        </div>
    </CardBody>
</Card>
