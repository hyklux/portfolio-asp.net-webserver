# portfolio-asp.net-webserver
ASP.NET 웹서버 포트폴리오

## 소개
REST API를 활용해 게임 결과 순위에 대한 CRUD를 수행하는 웹기반 API 서버입니다.

## 기능
:heavy_check_mark: 엔티티용 클래스 GameResult 생성


:heavy_check_mark: ApplicationDbContext 생성 및 GameResults DbSet 선언


:heavy_check_mark: REST API에 대한 응답을 처리할 Controller 생성


:heavy_check_mark: [Create] 게임 결과 생성하기


:heavy_check_mark: [Read] 게임 결과 불러오기


:heavy_check_mark: [Update] 게임 결과 수정하기


:heavy_check_mark: [Delete] 게임 결과 삭제하기


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
