# GitHub Actions Caching Strategy

## Overview

This document describes the comprehensive caching strategy implemented across all workflows to improve performance, reduce costs, and increase resilience.

## Benefits

### Performance Improvements
- **30-70% faster workflow execution** on cache hits
- **Reduced network I/O** by reusing downloaded dependencies
- **Lower latency** by avoiding repeated downloads from external registries

### Cost Savings
- **Reduced GitHub Actions minutes** through faster execution
- **Lower network transfer costs** by minimizing external downloads
- **Better resource utilization** of runner compute time

### Resilience Improvements
- **Reduced external dependencies** during workflow execution
- **Better handling of external service outages** (PyPI, RubyGems, Docker Hub)
- **Consistent build environment** across workflow runs

## Caching Implementation

### APT Package Cache

Caches Ubuntu system packages to speed up any apt installations performed by underlying actions.

```yaml
- name: Cache APT packages
  uses: actions/cache@9255dc7a253b0ccc959486e2bca901246202afeb # v5.0.1
  with:
    path: |
      /var/cache/apt/archives
      /var/lib/apt/lists
    key: ${{ runner.os }}-apt-${{ hashFiles('.github/workflows/*.yml') }}
    restore-keys: |
      ${{ runner.os }}-apt-
```

**Paths:**
- `/var/cache/apt/archives` - Downloaded .deb package files
- `/var/lib/apt/lists` - Package index files

**Cache Key Strategy:**
- Primary key includes workflow file hash for invalidation on changes
- Restore key allows fallback to any APT cache

**Benefits:**
- Faster installation of system dependencies
- Reduced load on Ubuntu package mirrors
- Better handling of mirror outages

### Python pip Cache

Caches Python packages for cfn-lint and checkov security scanning tools.

```yaml
- name: Setup Python
  uses: actions/setup-python@0b93645e9fea7318ecaed2b359559ac225c90a2b # v5.3.0
  with:
    python-version: '3.12'

- name: Cache Python dependencies
  uses: actions/cache@9255dc7a253b0ccc959486e2bca901246202afeb # v5.0.1
  with:
    path: |
      ~/.cache/pip
      ~/.local/lib/python3.12/site-packages
    key: ${{ runner.os }}-pip-cfn-lint-checkov-${{ hashFiles('**/requirements.txt', '.github/workflows/*.yml') }}
    restore-keys: |
      ${{ runner.os }}-pip-cfn-lint-checkov-
      ${{ runner.os }}-pip-
```

**Paths:**
- `~/.cache/pip` - pip download cache
- `~/.local/lib/python3.12/site-packages` - Installed Python packages

**Cache Key Strategy:**
- Primary key includes requirements.txt and workflow file hashes
- Multiple restore keys for progressive fallback
- Tool-specific prefix for better cache organization

**Benefits:**
- Faster cfn-lint CloudFormation validation
- Faster Checkov security scanning
- Reduced load on PyPI (files.pythonhosted.org)
- Better handling of PyPI outages

### Ruby Gems Cache

Caches Ruby gems for cfn-nag security scanning tool.

```yaml
- name: Setup Ruby
  uses: ruby/setup-ruby@f26937343756d1117ac1691f0f5c4bfc373a0a01 # v1.208.0
  with:
    ruby-version: '3.3'

- name: Cache Ruby gems
  uses: actions/cache@9255dc7a253b0ccc959486e2bca901246202afeb # v5.0.1
  with:
    path: vendor/bundle
    key: ${{ runner.os }}-gems-cfn-nag-${{ hashFiles('**/Gemfile.lock', '.github/workflows/*.yml') }}
    restore-keys: |
      ${{ runner.os }}-gems-cfn-nag-
      ${{ runner.os }}-gems-
```

**Paths:**
- `vendor/bundle` - Bundler gem installation directory

**Cache Key Strategy:**
- Primary key includes Gemfile.lock and workflow file hashes
- Multiple restore keys for progressive fallback
- Tool-specific prefix for better cache organization

**Benefits:**
- Faster cfn-nag static analysis
- Reduced load on RubyGems.org
- Better handling of RubyGems outages

### Docker Layer Cache (main.yml only)

Caches Docker image layers for ZAP API security scanning.

```yaml
- name: Set up Docker Buildx
  uses: docker/setup-buildx-action@c47758b77c9736f4b2ef4073d4d51994fabfe349 # v3.7.1

- name: Cache Docker layers
  uses: actions/cache@9255dc7a253b0ccc959486e2bca901246202afeb # v5.0.1
  with:
    path: /tmp/.buildx-cache
    key: ${{ runner.os }}-docker-zap-${{ hashFiles('.github/workflows/main.yml') }}
    restore-keys: |
      ${{ runner.os }}-docker-zap-
      ${{ runner.os }}-docker-
```

**Paths:**
- `/tmp/.buildx-cache` - Docker BuildKit cache directory

**Cache Key Strategy:**
- Primary key includes workflow file hash
- Multiple restore keys for progressive fallback
- Tool-specific prefix for better cache organization

**Benefits:**
- Faster ZAP Docker image pulls
- Reduced load on GitHub Container Registry (ghcr.io)
- Better handling of registry outages
- Faster security scanning execution

## Workflow Coverage

### main.yml (Deployment Workflow)
- ✅ APT package cache
- ✅ Python pip cache (cfn-lint, checkov)
- ✅ Ruby gems cache (cfn-nag)
- ✅ Docker layer cache (ZAP)

**Tools Accelerated:**
- scottbrenner/cfn-lint-action (4 instances)
- stelligent/cfn_nag (2 instances)
- bridgecrewio/checkov-action (1 instance)
- zaproxy/action-api-scan (1 instance)

### pullrequest.yml (PR Validation Workflow)
- ✅ APT package cache
- ✅ Python pip cache (cfn-lint, checkov)
- ✅ Ruby gems cache (cfn-nag)

**Tools Accelerated:**
- scottbrenner/cfn-lint-action (4 instances)
- stelligent/cfn_nag (1 instance)
- bridgecrewio/checkov-action (1 instance)

## Cache Invalidation Strategy

### When Caches Are Invalidated

1. **Workflow File Changes**: Cache key includes workflow file hash
2. **Dependency Changes**: Cache key includes requirements.txt, Gemfile.lock hashes
3. **OS Changes**: Cache key includes runner.os
4. **Manual Invalidation**: Update cache key prefix in workflow

### Automatic Fallback

All caches use progressive restore keys:
```yaml
key: ${{ runner.os }}-tool-specific-${{ hashFiles(...) }}
restore-keys: |
  ${{ runner.os }}-tool-specific-
  ${{ runner.os }}-
```

This allows:
1. Exact match → Use specific cache
2. Tool match → Use any version of tool cache
3. OS match → Use any cache for the OS
4. No match → Build from scratch

## Monitoring and Optimization

### Key Metrics to Monitor

1. **Cache Hit Rate**: Track percentage of successful cache restores
2. **Workflow Duration**: Measure execution time improvements
3. **Cache Size**: Monitor storage usage in GitHub Actions cache
4. **External Service Reliability**: Track dependency on external services

### Expected Performance

| Workflow | First Run | Cache Hit | Improvement |
|----------|-----------|-----------|-------------|
| main.yml | ~8-10 min | ~5-7 min | 30-40% faster |
| pullrequest.yml | ~5-7 min | ~3-4 min | 40-50% faster |

### Cache Size Limits

- **GitHub Actions Cache Limit**: 10 GB per repository
- **Current Cache Usage** (estimated):
  - APT packages: ~100-200 MB
  - Python pip: ~50-100 MB
  - Ruby gems: ~20-50 MB
  - Docker layers: ~500-800 MB
  - **Total**: ~700-1150 MB per workflow

### Troubleshooting

#### Cache Not Being Used

Check the workflow logs for:
```
Cache not found for input keys: Linux-pip-cfn-lint-checkov-abc123
```

**Solutions:**
1. Verify cache key hash inputs exist
2. Check if cache was evicted (7-day retention)
3. Verify workflow has correct cache action version

#### Cache Restore Failures

Check for permission errors:
```
Error: Unable to restore cache
```

**Solutions:**
1. Verify workflow has `contents: read` permission
2. Check if cache size exceeds 10 GB limit
3. Verify cache paths exist and are accessible

#### Slow Cache Operations

If cache save/restore is slow:
1. Reduce cache path sizes
2. Use more specific restore-keys
3. Consider cache compression settings

## Security Considerations

### Pinned Action Versions

All cache actions use SHA-pinned versions for security:
- `actions/cache@9255dc7a253b0ccc959486e2bca901246202afeb # v5.0.1`
- `actions/setup-python@0b93645e9fea7318ecaed2b359559ac225c90a2b # v5.3.0`
- `ruby/setup-ruby@f26937343756d1117ac1691f0f5c4bfc373a0a01 # v1.208.0`
- `docker/setup-buildx-action@c47758b77c9736f4b2ef4073d4d51994fabfe349 # v3.7.1`

### Cache Isolation

Caches are:
- Scoped to repository
- Isolated between branches (for PRs)
- Accessible within workflow runs
- Protected by GitHub Actions security model

### No Secrets in Cache

**Warning:** Never cache:
- Environment variables with secrets
- AWS credentials
- API tokens
- Private keys
- Configuration files with sensitive data

## Maintenance

### Regular Reviews

Review caching strategy:
- **Monthly**: Check cache hit rates and performance metrics
- **Quarterly**: Update pinned action versions
- **After Tool Updates**: Verify cache compatibility with new tool versions

### Updates Required When

1. **Tool Version Changes**: Update cache keys if tool versions change
2. **New Tools Added**: Add corresponding cache configuration
3. **Workflow Restructure**: Review and update cache key hashes
4. **Performance Issues**: Adjust cache paths or key strategy

## References

- [GitHub Actions Caching Documentation](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows)
- [actions/cache Repository](https://github.com/actions/cache)
- [Docker Buildx Cache Documentation](https://docs.docker.com/build/cache/)
- [pip Caching](https://pip.pypa.io/en/stable/topics/caching/)
- [Bundler Configuration](https://bundler.io/man/bundle-config.1.html)

---

**Last Updated**: 2025-12-16  
**Author**: DevOps Team  
**Version**: 1.0
