<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Movie 모델 스캐폴드

* 명령줄에서 다음을 실행합니다(*Program.cs*, *Startup.cs* 및 *.csproj* 파일이 포함된 프로젝트 디렉터리에서).

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

오류가 표시될 경우:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

프로젝트 디렉터리(*Program.cs*, *Startup.cs* 및 *.csproj* 파일이 포함된 디렉터리)에 대해 명령 셸을 엽니다.

오류가 표시될 경우:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Visual Studio를 종료하고 명령을 다시 실행합니다.
