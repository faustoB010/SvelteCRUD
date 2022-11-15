<script>
    import Card from "sveltestrap/src/Card.svelte";
    import CardBody from "sveltestrap/src/CardBody.svelte";
    import CardHeader from "sveltestrap/src/CardHeader.svelte";
    import Form from "sveltestrap/src/Form.svelte";
    import FormGroup from "sveltestrap/src/FormGroup.svelte";
    import Label from "sveltestrap/src/Label.svelte";
    import Input from "sveltestrap/src/Input.svelte";

    let idiomas = [];
    let activado = true;
    let datosIdioma = {
        idioma_id: null,
        idioma: "",
    };

    let mostrarIdioma = () => {
        fetch("https://localhost:44315/api/Idioma/")
            .then((respuesta) => respuesta.json())
            .then((datosIdioma) => {
                idiomas = datosIdioma;
                datosIdioma = {
                    idioma_id: null,
                    idioma: "",
                };
                console.log(idiomas);
            })
            .catch(console.log);
            
    };

    let agregarIdioma = () => {
        const nuevoIdioma = {
            idioma_id: null,
            idioma: datosIdioma.idioma,

        };
        fetch("https://localhost:44315/api/Idioma/", {
            method: "POST",
            headers: { "Content-type": "application/json; charset=UTF-8" },
            body: JSON.stringify(nuevoIdioma),
        })
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                console.log(nuevoIdioma);
                console.log(datosRespuesta);
                mostrarIdioma();
            })
            .catch(console.log);
            location.reload();
    };

    let borrarIdioma = (id) => {
        fetch("https://localhost:44315/api/Idioma/" + id, {
            method: "DELETE",
        })
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                mostrarIdioma();
            })
            .catch(console.log);
            location.reload();
            
    };

    let editarIdioma = (idioma) => {
        datosIdioma = idioma;
    };
    let actualizarIdioma = () => {
        fetch(
            "https://localhost:44315/api/Idioma/" + datosIdioma.idioma_id,
            {
                method: "PUT",
                headers: { "Content-type": "application/json; charset=UTF-8" },
                body: JSON.stringify(datosIdioma),
            },
        
        )
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                console.log(nuevoIdioma);
                console.log(datosRespuesta);
                mostrarIdioma();
            },alert("El idioma ha sido creado con éxito"),
                window.location.reload())
            .catch(console.log);
            
    };
    mostrarIdioma();
</script>

<h1 class="mt-4">Idiomas</h1>

<Card class="mb-4">
    <CardHeader>
        <i class="fas fa-table" />
        Idiomas
    </CardHeader>
    <CardBody>
        <div class="container text-center">
            <div class="table-responsive">
                <table class="table table-striped">
                    <thead>
                        <tr>
                            <th>Idioma ID</th>
                            <th>Idioma</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody>
                        {#each idiomas as idioma}
                            <tr>
                                <td>{idioma.idioma_id}</td>
                                <td>{idioma.idioma}</td>
                                <td>
                                    <FormGroup>
                                        <button
                                            on:click|preventDefault={editarIdioma(
                                                idioma
                                            )}
                                            type="submit"
                                            class="btn btn-secondary"
                                        >
                                            Editar
                                            
                                        </button>
                                    </FormGroup>
                                    <FormGroup>
                                        <button
                                            on:click|preventDefault={borrarIdioma}
                                            type="submit"
                                            class="btn btn-dark"
                                            on:click={borrarIdioma(
                                                idioma.idioma_id
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
                    <Label for="name" class="small mb-1">Idioma ID</Label>
                    <Input
                        readonly
                        type="number"
                        bind:value={datosIdioma.idioma_id}
                        class="label col-sm-3 col-form-label"
                        name="idioma_id"
                        placeholder="Idioma ID"
                    />
                </FormGroup>
                <FormGroup>
                    <Label for="name" class="small mb-1">Idioma</Label>
                    <Input
                        type="text"
                        bind:value={datosIdioma.idioma}
                        class="label col-sm-3 col-form-label"
                        name="idioma"
                        placeholder="Idioma"
                    />
                </FormGroup>
                
                
                <FormGroup>
                    <button
                        type="button"
                        class="btn btn-secondary"
                        on:click|preventDefault={agregarIdioma}
                        >Agregar</button
                    >
                    <button
                        type="button"
                        class="btn btn-dark"
                        on:click|preventDefault={actualizarIdioma}
                        >Actualizar</button
                    >
                </FormGroup>
            </Form>
        </div>
    </CardBody>
</Card>
