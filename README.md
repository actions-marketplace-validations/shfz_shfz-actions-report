# shfz-actions-report

This action for report shfz.

Create fuzzing report to issue, and upload fuzzing log to actions artifact.

## Inputs

## `version`

**Optional** application source code path.

## Outputs

## Example usage

```
uses: shfz/shfz-actions-report@v0.0.1
with:
  path: "/app"
```

## Includes actions

```yml
    - name: export fuzzing report
      run: >
        curl
        -F "hash=${{ github.sha }}"
        -F "repo=${{ github.repository }}"
        -F "id=${{ github.run_id }}"
        -F "job=${{ github.job }}"
        -F "number=${{ github.run_number }}"
        -F "path=${{ inputs.path }}"
        http://localhost:53653/report > report.md
      shell: bash
    - name: create issue
      uses: peter-evans/create-issue-from-file@v3
      with:
        title: shfz result
        content-filepath: ./report.md
        labels: |
          shfz
    - name: export fuzzing data
      run: curl http://localhost:53653/data > result.json
      shell: bash
    - name: upload artifact
      uses: actions/upload-artifact@v2
      with:
        name: result.json
        path: ./result.json
      shell: bash
```
