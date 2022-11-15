<script>
    import Card from "sveltestrap/src/Card.svelte";
    import CardBody from "sveltestrap/src/CardBody.svelte";
    import CardHeader from "sveltestrap/src/CardHeader.svelte";
    import Form from "sveltestrap/src/Form.svelte";
    import FormGroup from "sveltestrap/src/FormGroup.svelte";
    import Label from "sveltestrap/src/Label.svelte";
    import Input from "sveltestrap/src/Input.svelte";

    let generos = [];
    let activado = true;
    let datosGenero = {
        genero_id: null,
        genero: "",
    };

    let mostrarGenero = () => {
        fetch("https://localhost:44315/api/Genero/")
            .then((respuesta) => respuesta.json())
            .then((datosGenero) => {
                generos = datosGenero;
                datosGenero = {
                    genero_id: null,
                    genero: "",
                };
                console.log(generos);
            })
            .catch(console.log);
            
    };

    let agregarGenero = () => {
        const nuevoGenero = {
            genero_id: null,
            genero: datosGenero.genero,

        };
        fetch("https://localhost:44315/api/Genero/", {
            method: "POST",
            headers: { "Content-type": "application/json; charset=UTF-8" },
            body: JSON.stringify(nuevoGenero),
        })
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                console.log(nuevoGenero);
                console.log(datosGenero);
                mostrarGenero();
            })
            .catch(console.log);
            location.reload();
    };

    let borrarGenero = (id) => {
        fetch("https://localhost:44315/api/Genero/" + id, {
            method: "DELETE",
        })
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                mostrarGenero();
            })
            .catch(console.log);
            location.reload();
            
    };

    let editarGenero = (genero) => {
        datosGenero = genero;
    };
    let actualizarGenero = () => {
        fetch(
            "https://localhost:44315/api/Genero/" + datosGenero.genero_id,
            {
                method: "PUT",
                headers: { "Content-type": "application/json; charset=UTF-8" },
                body: JSON.stringify(datosGenero),
            },
            alert("El género ha sido actualizado con éxito")
        )
            .then((respuesta) => respuesta.json())
            .then((datosRespuesta) => {
                console.log(nuevoGenero);
                console.log(datosGenero);
                mostrarGenero();
                
            })
            .catch();

            location.reload();
    };
    mostrarGenero();
</script>

<h1 class="mt-4">Géneros</h1>

<Card class="mb-4">
    <CardHeader>
        <i class="fas fa-table" />
        Géneros
    </CardHeader>
    <CardBody>
        <div class="container text-center">
            <div class="table-responsive">
                <table class="table table-striped">
                    <thead>
                        <tr>
                            <th>Género ID</th>
                            <th>Género</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody>
                        {#each generos as genero}
                            <tr>
                                <td>{genero.genero_id}</td>
                                <td>{genero.genero}</td>
                                <td>
                                    <FormGroup>
                                        <button
                                            on:click|preventDefault={editarGenero(
                                                genero
                                            )}
                                            type="submit"
                                            class="btn btn-secondary"
                                            
                                        >
                                           Editar
                                            
                                        </button>
                                    </FormGroup>
                                    <FormGroup>
                                        <button
                                            on:click|preventDefault={borrarGenero}
                                            type="submit"
                                            class="btn btn-dark"
                                            on:click={borrarGenero(
                                                genero.genero_id
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
<Card class="mb-4" id="hola" >
    <CardHeader>
        <i class="fas fa-edit" />
        Crear/Editar
    </CardHeader>
    <CardBody>
        <div align="center">
            <Form>
                <FormGroup>
                    <Label for="name" class="small mb-1">Género ID</Label>
                    <Input
                        readonly
                        type="number"
                        bind:value={datosGenero.genero_id}
                        class="label col-sm-3 col-form-label"
                        name="genero_id"
                        placeholder="Género ID"
                    />
                </FormGroup>
                <FormGroup>
                    <Label for="name" class="small mb-1">Género</Label>
                    <Input
                        type="text"
                        bind:value={datosGenero.genero}
                        class="label col-sm-3 col-form-label"
                        name="genero"
                        placeholder="Género"
                    />
                </FormGroup>
                
                
                <FormGroup>
                    <button
                        type="button"
                        class="btn btn-secondary"
                        on:click|preventDefault={agregarGenero}
                        >Agregar</button
                    >
                    <button
                        type="button"
                        class="btn btn-dark"
                        on:click|preventDefault={actualizarGenero}
                        >Actualizar</button
                    >
                </FormGroup>
            </Form>
        </div>
    </CardBody>
</Card>
