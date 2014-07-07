require "pathname"

projects = %w[
test-unit
test-unit-activesupport
test-unit-capybara
test-unit-notify
test-unit-rails
test-unit-rr
]

base_dir = Pathname(__FILE__).realpath.dirname.parent
publish_base = Pathname(__FILE__).realpath.dirname

task :update do
  projects.each do |project|
    project_dir = base_dir + project
    Dir.chdir(project_dir.to_path) do
      sh "bundle", "exec", "rake", "reference:generate"
    end
    if project == "test-unit"
      html_dir = project_dir + "doc/html"
      sh "rsync -a #{html_dir}/ #{publish_base}/"
    end
    reference_dir = project_dir + "doc/reference"
    publish_dir = publish_base + project
    sh "rsync -a --delete #{reference_dir}/ #{publish_dir}/"
  end
end

task :publish => :update do
  sh "git", "add", "-u"
  sh "git", "commit", "-m", "Update documents"
  sh "git", "push"
end
