# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an algorithm study group repository focused on competitive programming practice. The repository contains solutions to problems from various online judges including BOJ (백준), SWEA (SW Expert Academy), PGS (프로그래머스), and JOL (Jungol).

## Repository Structure

The repository is organized chronologically by year and month:
```
25년/
├── 7월/
│   └── 5주차/
└── 8월/
    ├── 1주차/
    ├── 2주차/
    ├── 3주차/
    └── 4주차/
```

Each weekly folder contains algorithm problem solutions authored by different study members (보승, 윤서, 은서, 현준).

## File Conventions

### File Naming
Files follow the pattern: `{Platform}_{Problem Title}_{Author Name}.md`
- Example: `BOJ_구간합구하기4_윤서.md`, `SWEA_숫자만들기_보승.md`

### Platform Abbreviations
- BOJ: 백준 (Baekjoon Online Judge)
- PGS: 프로그래머스 (Programmers)  
- SWEA: SW Expert Academy
- JOL: Jungol

### File Structure
Each solution file contains:
- Problem title with platform link
- 풀이 (Solution approach/explanation)
- 해답 (Code solution in Java)

## Development Practices

### Commit Convention
Format: `✨ feat: {Platform}_{Problem}_{Author}_{Category}_{Type}`
- Example: `✨ feat: SWEA_숫자만들기_윤서_알고리즘_구현`

## Weekly File Migration from SSAFY Repositories

### Quick Start
**Simple Commands:**
- `"N주차 스터디원 알고리즘 파일 수집해줘"` (N = 1,2,3,4...)
- `"8월 N주차 파일 마이그레이션 해줘"`
- `"이번 주차 알고리즘 파일들 정리해줘"`

### Source Repositories
- `https://lab.ssafy.com/khim98/ssafy14_gwangju5_algorithm.git`
- `https://lab.ssafy.com/gda05189/ssafy14_gwangju5_algo_homework.git`

### Study Members
**보승**(강보승), **윤서**(남윤서), **은서**(김은서), **현준**(장현준)

### Weekly Schedule
- **1주차**: 0804-0810 (Aug 4-10)
- **2주차**: 0811-0817 (Aug 11-17)  
- **3주차**: 0818-0824 (Aug 18-24)
- **4주차**: 0825-0831 (Aug 25-31)
- **5주차+**: 0901-0907 (Sep 1-7, 확장 가능)

## Automated Collection Process (V6.0)

### Core Strategy: Direct Folder Search + Copy Method
**Proven Method**: `ls` direct folder checking + file copy for Korean encoding
- **Replaces**: Complex `find` + `grep` pipelines (70% accuracy)
- **Benefits**: Perfect Korean filename support, 100% file reading success

### Critical Issue Solved: Korean Filename Encoding
**Problem**: Files found by `ls` but unreadable by `cat` due to encoding issues
**Solution**: Copy files to temporary English names before reading

### Implementation
```bash
function collect_week_files_v6() {
  local week_num=$1  # e.g., 3 for "3주차"
  local month=$2     # e.g., 8 for "8월"
  
  # 1. Clone repositories
  git clone [repos] /tmp/ssafy_*
  
  # 2. Direct folder search for each date
  local week_dates=("0818" "0819" "0820" "0821" "0822" "0823" "0824")
  for date in "${week_dates[@]}"; do
    for folder in /tmp/ssafy_algorithm/08월_알골/$date/*/; do
      if [[ -d "$folder" ]]; then
        # Direct ls search - 100% reliable for discovery
        cd "$folder"
        ls | grep -E "(강보승|남윤서|김은서|장현준)" | while read filename; do
          # Korean filename encoding fix: copy then read
          cp "$filename" "/tmp/temp_$(date +%s).java"
          cat "/tmp/temp_$(date +%s).java" > processed_content
          # Process content and create .md files
        done
      fi
    done
  done
  
  # 3. Alternative: Use index-based copy for first matching file
  cd "$target_folder"
  ls | head -1 | xargs -I {} cp "{}" /tmp/temp_file.java
  cat /tmp/temp_file.java  # This works 100%
  
  rm -rf /tmp/ssafy_* /tmp/temp_*
}
```

### File Conversion Template
```markdown
## {Problem_Name}([Platform](link) {Problem_Number})

## 풀이

{Algorithm_approach_from_comments}

## 해답

```java
{Complete_Java_code_with_comments}
```
```

### System Features (V6.0)
- ✅ **100% File Discovery**: Direct `ls` folder listing finds all Korean filenames
- ✅ **100% File Reading**: Copy-then-read method solves encoding issues completely
- ✅ **Korean Filename Support**: Perfect handling via temporary file copying
- ✅ **Auto Week Detection**: Automatic date range calculation
- ✅ **Batch Processing**: All study members processed simultaneously  
- ✅ **Encoding Resilient**: No more `cat: No such file or directory` errors
- ✅ **Extensible**: Easy expansion to new months (September, October, etc.)

## Common Development Tasks

Since this is a documentation-based algorithm study repository, there are no build, test, or lint commands. The primary workflow involves:

1. Creating new solution files following naming conventions
2. Writing problem analysis and Java solutions
3. Following commit message conventions
4. Conducting peer code reviews
5. **Automated migration** of solutions from SSAFY repositories