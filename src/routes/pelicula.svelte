<script>
    import Card from "sveltestrap/src/Card.svelte";
    import CardBody from "sveltestrap/src/CardBody.svelte";
    import CardHeader from "sveltestrap/src/CardHeader.svelte";
    import Form from "sveltestrap/src/Form.svelte";
    import FormGroup from "sveltestrap/src/FormGroup.svelte";
    import Label from "sveltestrap/src/Label.svelte";
    import Input from "sveltestrap/src/Input.svelte";

    let peliculas = [];

    let activado = true;
    let idiomaSeleccionado;
    let generoSeleccionado;
    let idiomas = [];
    let generos = [];
    let datosPelicula = {
        pelicula_id: null,
        titulo: "",
        genero_id: "",
        idioma_id: "",
        director: "",
        duracion_minutos: "",
        anio: "",
        precio: "",
    };

    let mostrarPelicula = () => {
        fetch("https://localhost:44315/api/Pelicula/")
            .then((respuesta) => respuesta.json())
            .then((datosPelicula) => {
                peliculas = datosPelicula;
                datosPelicula = {
                    pelicula_id: null,
                    titulo: "",
                    genero_id: "",
                    idioma_id: "",
                    director: "",
                    duracion_minutos: "",
                    anio: "",
                    precio: "",
                };
                console.log(peliculas);
            })
            .catch(console.log);
    };

    let agregarPelicula = () => {
        var comboGenero = document.getElementById("comboGenero");
        var comboIdioma = document.getElementById("comboIdiomas");

        datosPelicula.genero_id = retornarIDGen(
            comboGenero.options[comboGenero.selectedIndex].text
        );

        datosPelicula.idioma_id = retornarIDIdm(
            comboIdioma.options[comboIdioma.selectedIndex].text
        );
        const nuevaPelicula = {
            pelicula_id: null,
            titulo: datosPelicula.titulo,
            genero_id: datosPelicula.genero_id,
            idioma_id: datosPelicula.idioma_id,
            director: datosPelicula.director,
            duracion_minutos: datosPelicula.duracion_minutos,
            anio: datosPelicula.anio,
            precio: datosPelicula.precio,
        };
        fetch("https://localhost:44315/api/Pelicula/", {
            method: "POST",
            headers: { "Content-type": "application/json; charset=UTF-8" },
            body: JSON.stringify(nuevaPelicula),
        })
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                console.log(nuevaPelicula);
                console.log(datosRespuesta);
                
                mostrarPelicula();
            },alert("La película ha sido creada con éxito"),
                window.location.reload())
            .catch(console.log);
    };

    let borrarPelicula = (id) => {
        fetch("https://localhost:44315/api/Pelicula/" + id, {
            method: "DELETE",
        })
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                mostrarPelicula();
            },alert("La película ha sido borrada con éxito"),
                window.location.reload())
            .catch(console.log);

    };

    let editarPelicula = (pelicula) => {
        datosPelicula = pelicula;
        var comboIdioma = document.getElementById("comboIdiomas");
        var comboGenero = document.getElementById("comboGenero");
        const gen = retornarGenDesc(datosPelicula.genero_id);
        const idm = retornarIdmDesc(datosPelicula.idioma_id);

        gen.then(
            (json) =>
                (comboGenero.options[comboGenero.selectedIndex].text =
                    json.genero)
        );

        idm.then(
            (json) =>
                (comboIdioma.options[comboIdioma.selectedIndex].text =
                    json.idioma)
        );
    };
    let actualizarPelicula = () => {
        var comboGenero = document.getElementById("comboGenero");
        var comboIdioma = document.getElementById("comboIdiomas");

        datosPelicula.genero_id = retornarIDGen(
            comboGenero.options[comboGenero.selectedIndex].text
        );

        datosPelicula.idioma_id = retornarIDIdm(
            comboIdioma.options[comboIdioma.selectedIndex].text
        );

        fetch(
            "https://localhost:44315/api/Pelicula/" + datosPelicula.pelicula_id,
            {
                method: "PUT",
                headers: { "Content-type": "application/json; charset=UTF-8" },
                body: JSON.stringify(datosPelicula),
            },
        )
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                console.log(nuevaPelicula);
                console.log(datosRespuesta);
                mostrarPelicula();
            },alert("La película ha sido editada con éxito"),
                window.location.reload())
            .catch(console.log);
    };
    async function retornarGenDesc(id) {
        const response = await fetch(
            "https://localhost:44315/api/Genero/" + id
        );

        return response.json();
    }
    async function retornarIdmDesc(id) {
        const response = await fetch(
            "https://localhost:44315/api/Idioma/" + id
        );

        return response.json();
    }

    function cargarGeneros() {
        fetch("https://localhost:44315/api/Genero/")
            .then((respuesta) => respuesta.json())
            .then((datosGenero) => {
                generos = datosGenero;
            })
            .catch(console.log);
    }

    function cargarIdiomas() {
        fetch("https://localhost:44315/api/Idioma/")
            .then((respuesta) => respuesta.json())
            .then((datosIdioma) => {
                idiomas = datosIdioma;
            })
            .catch(console.log);
    }
    function retornarIDGen(gen_id) {
        let respuesta = 0;

        for (let i = 0; i < generos.length; i++) {
            let a = generos[i];

            if (a.genero === gen_id) {
                respuesta = a.genero_id;
            }
        }
        return respuesta;
    }

    function retornarIDIdm(idiom_id) {
        let respuesta = 0;

        for (let i = 0; i < idiomas.length; i++) {
            let a = idiomas[i];

            if (a.idioma === idiom_id) {
                respuesta = a.idioma_id;
            }
        }
        return respuesta;
    }

    cargarIdiomas();
    cargarGeneros();
    mostrarPelicula();
</script>

<h1 class="mt-4">Películas</h1>

<Card class="mb-4">
    <CardHeader>
        <i class="fas fa-table" />
        Películas
    </CardHeader>
    <CardBody>
        <div class="container text-center">
            <div class="table-responsive">
                <table class="table table-striped">
                    <thead>
                        <tr>
                            <th>Película ID</th>
                            <th>Titulo</th>
                            <th>Género</th>
                            <th>Idioma</th>
                            <th>Director</th>
                            <th>Duracion minutos</th>
                            <th>Año</th>
                            <th>Precio</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody>
                        {#each peliculas as pelicula}
                            <tr>
                                <td>{pelicula.pelicula_id}</td>
                                <td>{pelicula.titulo}</td>
                                {#await retornarGenDesc(pelicula.genero_id)}
                                    <p>cargando....</p>
                                {:then data}
                                    <td>{data.genero}</td>
                                {:catch error}
                                    <p>Algo no ha ido bien!XXXX{error}</p>
                                {/await}
                                {#await retornarIdmDesc(pelicula.idioma_id)}
                                    <p>cargando....</p>
                                {:then data}
                                    <td>{data.idioma}</td>
                                {:catch error}
                                    <p>Algo no ha ido bien!{error}</p>
                                {/await}
                                <td>{pelicula.director}</td>
                                <td>{pelicula.duracion_minutos}</td>
                                <td>{pelicula.anio}</td>
                                <td>{pelicula.precio}</td>
                                <td>
                                    <FormGroup>
                                        <button
                                            on:click|preventDefault={editarPelicula(
                                                pelicula
                                            )}
                                            type="submit"
                                            class="btn btn-secondary"
                                        >
                                            Editar
                                        </button>
                                    </FormGroup>
                                    <FormGroup>
                                        <button
                                            on:click|preventDefault={borrarPelicula}
                                            type="submit"
                                            class="btn btn-dark"
                                            on:click={borrarPelicula(
                                                pelicula.pelicula_id
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
<Card class="mb-4">
    <CardHeader>
        <i class="fas fa-edit" />
        Crear/Editar
    </CardHeader>
    <CardBody>
        <div align="center">
            <Form>
                <FormGroup>
                    <Label for="name" class="small mb-1">Película ID</Label>
                    <Input
                        readonly
                        type="number"
                        bind:value={datosPelicula.pelicula_id}
                        class="label col-sm-3 col-form-label"
                        name="pelicula_id"
                        placeholder="Película ID"
                    />
                </FormGroup>
                <FormGroup>
                    <Label for="name" class="small mb-1">Título</Label><br />
                    <Input
                        type="text"
                        bind:value={datosPelicula.titulo}
                        class="label col-sm-3 col-form-label"
                        name="titulo_id"
                        placeholder="Título"
                    />
                </FormGroup>
                <br />
                <FormGroup>
                    <Label for="name" class="small mb-1">Idiomas</Label><br />
                    <select
                        class="label col-sm-3 col-form-label"
                        value={idiomaSeleccionado}
                        id="comboIdiomas"
                        on:change={cargarIdiomas}
                    >
                        {#each idiomas as idm}
                            <option value={idm.idioma_id}>
                                {idm.idioma}
                            </option>
                        {/each}
                    </select>
                </FormGroup>
                <br />
                <FormGroup>
                    <Label for="name" class="small mb-1">Generos</Label><br />
                    <select
                        class="label col-sm-3 col-form-label"
                        value={generoSeleccionado}
                        id="comboGenero"
                        on:change|once={cargarGeneros}
                    >
                        {#each generos as gen}
                            <option value={gen.genero_id}>
                                {gen.genero}
                            </option>
                        {/each}
                    </select>
                </FormGroup>
                <br />

                <FormGroup>
                    <Label for="name" class="small mb-1">Director</Label>
                    <Input
                        type="text"
                        bind:value={datosPelicula.director}
                        class="label col-sm-3 col-form-label"
                        name="director"
                        placeholder="Director"
                    />
                </FormGroup>
                <FormGroup>
                    <Label for="name" class="small mb-1">Duracion minutos</Label
                    >
                    <Input
                        type="text"
                        bind:value={datosPelicula.duracion_minutos}
                        class="label col-sm-3 col-form-label"
                        name="duracion_minutos"
                        placeholder="Duracion minutos"
                    />
                </FormGroup>
                <FormGroup>
                    <Label for="name" class="small mb-1">Año</Label>
                    <Input
                        type="date"
                        bind:value={datosPelicula.anio}
                        class="label col-sm-3 col-form-label"
                        name="anio"
                        placeholder="Año"
                    />
                </FormGroup>
                <FormGroup>
                    <Label for="name" class="small mb-1">Precio</Label>
                    <Input
                        type="number"
                        bind:value={datosPelicula.precio}
                        class="label col-sm-3 col-form-label"
                        name="precio"
                        placeholder="Precio"
                    />
                </FormGroup>
                <FormGroup>
                    <button
                        type="button"
                        class="btn btn-secondary"
                        on:click|preventDefault={agregarPelicula}
                        >Agregar</button
                    >
                    <button
                        type="button"
                        class="btn btn-dark"
                        on:click|preventDefault={actualizarPelicula}
                        >Actualizar</button
                    >
                </FormGroup>
            </Form>
        </div>
    </CardBody>
</Card>
