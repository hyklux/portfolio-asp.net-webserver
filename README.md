# portfolio-asp.net-webserver
ASP.NET 웹서버 포트폴리오

## 소개
REST API를 활용해 게임 결과 순위에 대한 CRUD를 수행하는 웹기반 API 서버입니다.

## 기능
:heavy_check_mark: 엔티티용 클래스 GameResult 생성


:heavy_check_mark: ApplicationDbContext 생성 및 GameResults DbSet 선언


:heavy_check_mark: REST API에 대한 응답을 처리할 Controller 생성


:heavy_check_mark: 게임 결과 생성하기(Create)


:heavy_check_mark: 모든 게임 결과 불러오기(Read)


:heavy_check_mark:  게임 결과 불러오기(Read)


:heavy_check_mark: 게임 결과 수정하기(Update)


:heavy_check_mark: 게임 결과 삭제하기(Delete)


## 엔티티용 클래스 GameResult 생성
``` c#
namespace SharedData.Models
{
	public class GameResult
	{
		public int Id { get; set; }
		public int UserId { get; set; }
		public string UserName { get; set; }
		public int Score { get; set; }
		public DateTime Date { get; set; }
	}
}
```


## ApplicationDbContext 생성 및 GameResults DbSet 선언
``` c#
public class ApplicationDbContext : DbContext
{
	public DbSet<GameResult> GameResults { get; set; }

	public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
		: base(options)
	{

	}
}
```


## REST API에 대한 응답을 처리할 Controller 생성
``` c#
[Route("api/[controller]")]
[ApiController]
public class RankingController : ControllerBase
{
	ApplicationDbContext _context;

	public RankingController(ApplicationDbContext context)
	{
		_context = context;
	}
}
```


## 게임 결과 생성하기(Create)
``` c#
// Create
// 아이템 생성 요청 (Body에 실제 정보)
// POST /api/ranking
[HttpPost]
public GameResult AddGameResult([FromBody] GameResult gameResult)
{
	_context.GameResults.Add(gameResult);
	_context.SaveChanges();

	return gameResult;
}
```


## 모든 게임 결과 불러오기(Read)
``` c#
// Read
// 모든 아이템
// GET /api/ranking
[HttpGet]
public List<GameResult> GetGameResults()
{
	List<GameResult> results = _context.GameResults
		.OrderByDescending(item => item.Score)
		.ToList();

	return results;
}
```


## 특정 게임 결과 불러오기(Read)
``` c#
// Read
// id=1번인 아이템
// GET /api/ranking/1
[HttpGet("{id}")]
public GameResult GetGameResult(int id)
{
	GameResult result = _context.GameResults
				.Where(item => item.Id == id)
				.FirstOrDefault();

	return result;
}
```


## 게임 결과 수정하기(Update)
``` c#
// Update
// 아이템 갱신 요청 (Body에 실제 정보)
// PUT /api/ranking (PUT 보안 문제로 웹에선 활용 X)
[HttpPut]
public bool UpdateGameResult([FromBody] GameResult gameResult)
{
	var findResult = _context.GameResults
		.Where(x => x.Id == gameResult.Id)
		.FirstOrDefault();

	if (findResult == null)
		return false;

	findResult.UserName = gameResult.UserName;
	findResult.Score = gameResult.Score;
	_context.SaveChanges();

	return true;
}
```


## 게임 결과 삭제하기(Delete)
``` c#
// Delete
// id=1번인 아이템 삭제
// DELETE /api/ranking/1 (DELETE 보안 문제로 웹에서 활용 X)
[HttpDelete("{id}")]
public bool DeleteGameResult(int id)
{
	var findResult = _context.GameResults
				.Where(x => x.Id == id)
				.FirstOrDefault();

	if (findResult == null)
		return false;

	_context.GameResults.Remove(findResult);
	_context.SaveChanges();

	return true;
}
```
