# runtime-image


      #  Use below cmd to stop all jobs
      # Get-Job | Stop-Job -Force
      
      1..100 | ForEach-Object {
          $index = $_
          Start-Job -ScriptBlock {
              $fileName = "ideaIU_$index.exe"
              $result = Invoke-WebRequest -Uri "https://download-cdn.jetbrains.com/idea/ideaIU-2024.1.1.exe" -OutFile "C:\mnt\iso\$fileName"
              [PSCustomObject]@{
                  Result = $result
                  Filename = $fileName
                  Timestamp = Get-Date
              }
          }
      } | Wait-Job | Receive-Job | Out-File "$PSScriptRoot\download_output.log"

# symultanious

            # Start 100 background jobs to download IntelliJ IDEA installer files concurrently
            1..100 | ForEach-Object {
                $index = $_
                Start-Job -ScriptBlock {
                    $fileName = "ideaIU_$index.exe"  # Construct unique filename
                    $result = Invoke-WebRequest -Uri "https://download-cdn.jetbrains.com/idea/ideaIU-2024.1.1.exe" -OutFile "C:\mnt\iso\$fileName"  # Download IntelliJ IDEA installer with unique filename
                    [PSCustomObject]@{
                        Result = $result  # Store result of download operation
                        Filename = $fileName  # Store filename
                        Timestamp = Get-Date  # Store timestamp of download
                    }
                }
            }
            
            # No need to wait for jobs to complete
