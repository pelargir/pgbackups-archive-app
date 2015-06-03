require "newrelic_rpm"
require "pgbackups-archive"


class RunPgbackupsArchive
  include NewRelic::Agent::Instrumentation::ControllerInstrumentation

  def call
    PgbackupsArchive::Job.call
  end

  add_transaction_tracer :call, category: :task
end

namespace :pgbackups do

  desc "Capture a Heroku PGBackups backup and archive it to Amazon S3."
  task :archive do
    RunPgbackupsArchive.new.call
  end

end

NewRelic::Agent.manual_start app_name: "pgbackups-archive-dummy",
  transaction_tracer: { transaction_threshold: 1.5 }
