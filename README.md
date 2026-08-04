var builder = WebApplication.CreateBuilder(args);

builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAll", policy =>
    {
        policy.AllowAnyOrigin()
              .AllowAnyMethod()
              .AllowAnyHeader();
    });
});

builder.WebHost.UseUrls("http://0.0.0.0:8000");

var app = builder.Build();

app.UseCors("AllowAll");

Funcionario[] funcionarios = new Funcionario[100];
int totalFuncionarios = 0;

app.MapGet("/", () =>
{
    return Results.Ok("API SkyMoon funcionando com sucesso!");
});

{
    Random random = new();

    Funcionario novo_funcionario = new Funcionario();

    novo_funcionario.Id = random.Next(1000, 9999);
    novo_funcionario.Nome = body.GetProperty("nome").GetString();
    novo_funcionario.Idade = body.GetProperty("idade").GetInt32();
    novo_funcionario.Cargo = body.GetProperty("cargo").GetString();
    novo_funcionario.Departamento = body.GetProperty("departamento").GetString();
    novo_funcionario.Salario = body.GetProperty("salario").GetDouble();

    funcionarios[totalFuncionarios] = novo_funcionario;
    totalFuncionarios++;

    return Results.Ok(new
    {
        novo_funcionario
    });
    
});

{
    Funcionario[] funcionariosCadastrados = new Funcionario[totalFuncionarios];

    for (int i = 0; i < totalFuncionarios; i++)
    {
        funcionariosCadastrados[i] = funcionarios[i];
    }

    return Results.Ok(new
    {
        funcionariosCadastrados
    });
});

{
     double novo_salario = body.GetProperty("salario").GetDouble();

    for (int i = 0; i < totalFuncionarios; i++)
    {
        if (funcionarios[i].Id == id)
        {
            funcionarios[i].Salario = novo_salario;
            return Results.Ok(
                new
                {
                    funcionario = funcionarios[i]
                }
            );
        }
    }

    return Results.NotFound(new
    {
        message = "Funcionário não encontrado."
    });
});

{   
    for (int i = 0; i < totalFuncionarios; i++)
    {
        if (funcionarios[i].Id == id)
        {
            funcionarios[i].Nome = body.GetProperty("nome").GetString();
            funcionarios[i].Idade = body.GetProperty("idade").GetInt32();
            funcionarios[i].Cargo = body.GetProperty("cargo").GetString();
            funcionarios[i].Departamento = body.GetProperty("departamento").GetString();
            funcionarios[i].Salario = body.GetProperty("salario").GetDouble();

            return Results.Ok(
                new
                {
                    funcionario = funcionarios[i]
                }
            );
        }
    }

    return Results.NotFound(new
    {
        message = "Funcionário não encontrado."
    });
});

{
     for (int i = 0; i < totalFuncionarios; i++)
    {
        if (funcionarios[i].Id == id)
        {
            Funcionario funcionarioRemovido = funcionarios[i];
            
            for (int j = i; j < totalFuncionarios - 1; j++)
            {
                funcionarios[j] = funcionarios[j + 1];
            }            

            totalFuncionarios--;

            return Results.Ok(new
            {
                mensagem = "Funcionário removido com sucesso.",
                funcionario = funcionarioRemovido
            });
        }
    }

    return Results.NotFound(new
    {
        message = "Funcionário não encontrado."
    });
});

{
    Funcionario[] funcionariosEncontrados = new Funcionario[totalFuncionarios];

    int totalEncontrados = 0;

    for (int i = 0; i < totalFuncionarios; i++)
    {
        if (funcionarios[i].Departamento.ToLower() == departamento.ToLower())
        // if (funcionarios[i].Departamento.ToLower().Equals(departamento, StringComparison.CurrentCultureIgnoreCase))
        {
            funcionariosEncontrados[totalEncontrados] = funcionarios[i];
            totalEncontrados++;
        }
    }

    {
        Funcionario[] resultadoFinal = new Funcionario[totalEncontrados];

        for (int i = 0; i < totalEncontrados; i++)
        {
            resultadoFinal[i] = funcionariosEncontrados[i];
        }        

        return Results.Ok(new
        {
            departamento,
            funcionarios = funcionariosEncontrados
        });
    } 

    return Results.NotFound(new
    {
        message = "Nenhum funcionário encontrado para esse departamento."
    });
});

{
   Funcionario[] funcionariosEncontrados = new Funcionario[totalFuncionarios];

    int totalEncontrados = 0;

    for (int i = 0; i < totalFuncionarios; i++)
    {
        if (funcionarios[i].Nome.ToLower() == nome.ToLower())
        {
            funcionariosEncontrados[totalEncontrados] = funcionarios[i];
            totalEncontrados++;
        }
    }

    {
        Funcionario[] resultadoFinal = new Funcionario[totalEncontrados];

        for (int i = 0; i < totalEncontrados; i++)
        {
            resultadoFinal[i] = funcionariosEncontrados[i];
        }        

        return Results.Ok(new
        {
            nome,
            funcionarios = funcionariosEncontrados
        });
    } 

    return Results.NotFound(new
    {
        message = "Nenhum funcionário encontrado esse nome."
    });
}); */

app.Run()
