# Don't let testing shortcuts get into master by accident

(modified_files + added_files - %w(Dangerfile)).each do |file|
  next unless File.file?(file)
  contents = File.read(file)
  if file.start_with?('spec')
    fail("`xit` or `fit` left in tests (#{file})") if contents =~ /^\w*[xf]it/
    fail("`fdescribe` left in tests (#{file})") if contents =~ /^\w*fdescribe/
  end
end

# Ensure a clean commits history
if commits.any? { |c| c.message =~ /^Merge branch '#{branch_for_merge}'/ }
  fail("Please rebase to get rid of the merge commits in this PR")
end

# Request a CHANGELOG entry, and give an example
declared_trivial = pr_title.include?("#trivial") || pr_body.include?("#trivial")
if !modified_files.include?("CHANGELOG.md") && !declared_trivial
  fail "Please include a CHANGELOG entry to credit yourself! \nYou can find it at [CHANGELOG.md](https://github.com/CocoaPods/CocoaPods/blob/master/CHANGELOG.md)."

  pr = env.request_source.pr_json
  markdown <<-MARKDOWN
Here's an example of your CHANGELOG entry:

```
* #{pr.title}  
  [#{pr_author}](https://github.com/#{pr_author})
  [##{pr.number}](https://github.com/#{pr.base.repo.full_name}/pull/#{pr.number})
```

*note*: There are two invisible spaces after the entry's text.
MARKDOWN
end
