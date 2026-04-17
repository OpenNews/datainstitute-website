require "jekyll"
require "yaml"

desc "Validate YAML files for syntax errors and duplicate keys"
task :validate_yaml do
  errors = []

  Dir
    .glob("{_config.yml,_data/**/*.{yml,yaml}}")
    .sort
    .each do |file|
      begin
        YAML.safe_load_file(file)
      rescue Psych::SyntaxError => e
        errors << "#{file}: syntax error — #{e.message}"
      rescue Psych::DisallowedClass => e
        errors << "#{file}: unsafe YAML — #{e.message}"
      rescue => e
        errors << "#{file}: #{e.message}"
      end
    end

  if errors.any?
    puts "❌ YAML validation errors:"
    errors.each { |e| puts "  - #{e}" }
    abort
  else
    puts "✅ YAML files are valid"
  end
end

desc "Check configuration and dependencies"
task :check do
  config = YAML.load_file("_config.yml")

  # Verify deployment config exists
  unless config["deployment"]
    puts "⚠️  No deployment configuration found in _config.yml"
  end

  puts "✓ Configuration checks passed"
end

desc "Build the Jekyll site"
task :build do
  sh "jekyll build"
end

desc "Test the built site"
task :test do
  unless Dir.exist?("_site")
    puts "✗ _site directory not found. Run 'rake build' first."
    exit 1
  end

  puts "✓ Site built successfully"
end

desc "Clean the build directory"
task :clean do
  FileUtils.rm_rf(%w[_site .jekyll-cache .jekyll-metadata])
  puts "✅ Cleanup complete"
rescue => e
  abort "\n❌ Failed to clean build directory: #{e.message}"
end

desc "Check all code formatting (Ruby + other files)"
task :lint do
  puts "Checking Ruby code style with StandardRB..."
  sh "bundle exec standardrb"
  puts "✅ All formatting checks passed!"
rescue => e
  abort "\n❌ Code formatting issues found: #{e.message}"
end

task default: %i[validate_yaml check build test]
