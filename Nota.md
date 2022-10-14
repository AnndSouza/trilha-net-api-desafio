- O que foi feito nesse desafio

## Projeto Tarefas
- package api entity framework já incluso

- Conexão Padrão - (link com o banco - homologação)


- Migration 
    - server ok
    - $dotnet-ef migrations add CriacaoTabelaTarefa 
    Aplicando a migration
        - $dotnet-ef database update

### Representação - (TarefaController)

## ObterPorId
      [HttpGet("{id}")]
        public IActionResult ObterPorId(int id)
        {
            var tarefa = _context.Tarefas.Find(id);
            if(tarefa == null)
                return NotFound();

            
            // TODO: Buscar o Id no banco utilizando o EF
            // TODO: Validar o tipo de retorno. Se não encontrar a tarefa, retornar NotFound,
            // caso contrário retornar OK com a tarefa encontrada
            return Ok(tarefa);
        }

## ObterTodos
    [HttpGet("ObterTodos")]
        public IActionResult ObterTodos()
        {

            var tarefas = _context.Tarefas.ToList() ;

            // TODO: Buscar todas as tarefas no banco utilizando o EF
            return Ok(tarefas);
        }
## ObterPorTitulo
     [HttpGet("ObterPorTitulo")]
        public IActionResult ObterPorTitulo(string titulo)
        {
            var tarefas = _context.Tarefas.Where(x => x.Titulo.Contains(titulo));
            
            // TODO: Buscar  as tarefas no banco utilizando o EF, que contenha o titulo recebido por parâmetro
            // Dica: Usar como exemplo o endpoint ObterPorData
            return Ok(tarefas);
        }

## TODO: Adicionar a tarefa recebida no EF e salvar as mudanças (save changes).

    public IActionResult Criar(Tarefa tarefa)
    _context.Add(tarefa);
    _context.SaveChanges();

## ObterPorStatus
     [HttpGet("ObterPorStatus")]
        public IActionResult ObterPorStatus(EnumStatusTarefa status)
        {
            
            // TODO: Buscar  as tarefas no banco utilizando o EF, que contenha o status recebido por parâmetro
            // Dica: Usar como exemplo o endpoint ObterPorData
            var tarefas = _context.Tarefas.Where(x => x.Status == status);
            return Ok(tarefas);
        }

## Método Criar
    [HttpPost]
        public IActionResult Criar(Tarefa tarefa)
        {
            if (tarefa.Data == DateTime.MinValue)
                return BadRequest(new { Erro = "A data da tarefa não pode ser vazia" });

            _context.Add(tarefa);//
            _context.SaveChanges();//

            // TODO: Adicionar a tarefa recebida no EF e salvar as mudanças (save changes) - ok
            return CreatedAtAction(nameof(ObterPorId), new { id = tarefa.Id }, tarefa);
        }

## Método Atualizar
    [HttpPut("{id}")]
        public IActionResult Atualizar(int id, Tarefa tarefa)
        {
            var tarefaBanco = _context.Tarefas.Find(id);

            if (tarefaBanco == null)
                return NotFound();

            if (tarefa.Data == DateTime.MinValue)
                return BadRequest(new { Erro = "A data da tarefa não pode ser vazia" });

            tarefaBanco.Data = tarefa.Data;
            tarefaBanco.Descricao = tarefa.Descricao;
            tarefaBanco.Status = tarefa.Status;
            tarefaBanco.Titulo = tarefa.Titulo;

            _context.Tarefas.Update(tarefaBanco);
            _context.SaveChanges();

## Método Deletar
    [HttpDelete("{id}")]
        public IActionResult Deletar(int id)
        {
            var tarefaBanco = _context.Tarefas.Find(id);

            if (tarefaBanco == null)
                return NotFound();

            _context.Tarefas.Remove(tarefaBanco);
            _context.SaveChanges();
            // TODO: Remover a tarefa encontrada através do EF e salvar as mudanças (save changes)
            return NoContent();
        }